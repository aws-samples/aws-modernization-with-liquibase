---
title: "Snapshot" # MODIFY THIS TITLE
chapter: true
weight: 71 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Database Snapshot

Liquibase provides a way to capture the current state of the database using the "__snapshot__" command. The snapshot is saved to disk using either a JSON or a YAML format.

Let us configure our flow file to capture a snapshot after the database update stage. 

![Snapshot stage](/images/lab5_drift_detection/flow_file_w_snapshot.png?width=400px&classes=border,shadow) 


## sqlcode/flow.yaml

We will modify the existing Liquibase Flow file in the GitHub repository, __sqlcode/flow.yaml__. 

Add the following new stage after the __Update__ stage and before the __endStage__. Your __Snapshot__ stage should look like this:

```yaml
...
...
...
  Update:
    actions:
      - type: liquibase
        command: update

  Snapshot:
    actions:
      - type: liquibase
        command: snapshot
        globalArgs: { output-file: "s3://liquibase-workshop-bucket/${BRANCH}-snapshots/mySnapshot.json" }
        cmdArgs: { snapshot-format: "JSON"}

endStage:
  actions:
    - type: liquibase
      command: history
```

## .github/workflows/aws.yml

Next, we will modify the __.github/workflows/aws.yml__ file provided in the GitHub repository to pass BRANCH environment variable to the flow file. We will set as `BRANCH=${{ github.ref_name }}`. Here is how your environment variable should look like in the aws.yml file.

```yaml
...
...
    - name: Fill in the new image ID in the Amazon ECS task definition
      id: render-task-def
      uses: aws-actions/amazon-ecs-render-task-definition@v1
      with:
        task-definition: ${{ env.ECS_TASK_DEFINITION }}
        container-name: ${{ env.CONTAINER_NAME }}
        image: ${{ env.IMAGE_NAME }}
        environment-variables: |
          LIQUIBASE_COMMAND_URL=${{ vars.LIQUIBASE_COMMAND_URL }}
          LIQUIBASE_COMMAND_USERNAME=${{ vars.LIQUIBASE_COMMAND_USERNAME }}
          LIQUIBASE_COMMAND_PASSWORD=${{ vars.LIQUIBASE_COMMAND_PASSWORD }}
          LIQUIBASE_COMMAND_TAG=${{ env.TAG }}
          LIQUIBASE_REPORTS_PATH=s3://liquibase-workshop-bucket/reports/update-${{ env.TAG }}/
          LIQUIBASE_COMMAND_CHECKS_RUN_REPORT_NAME=CHECKS-${{ env.TAG }}.html
          LIQUIBASE_COMMAND_UPDATE_REPORT_NAME=UPDATE-${{ env.TAG }}.html
          BRANCH=${{ github.ref_name }}
...
...
```

## Commit Change

Let's commit our changes to the GitHub repository.

1. Let's make sure that you are on the "dev" branch and fetch latest changes. In your command line terminal, run these commands:
    ```shell
    git checkout dev
    git fetch origin
    ```

1. Add, commit and push changes to the "dev" branch.
    ```shell
    git add .
    git commit -m "snapshot added"
    git push -u origin dev
    ```

The __git push__ command will automatically trigger a workflow in GitHub Actions.

## Validate that the GitHub Actions workflow ran and your snapshot was added to the S3 bucket

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "ECS". Then click "Elastic Container Service" under Services:
![Console home](/images/lab5_drift_detection/aws_console_home_1.png?width=600px&classes=border,shadow)

1. Click on "Clusters" in the left pane. Notice that if there is a task running on this cluster it will indicate as _pending_ or _running_. Click "workshop-cluster" to get to the details of this running task.
![AWS ECS Cluster](/images/lab5_drift_detection/aws_ecs_cluster_1.png?width=800px&classes=border,shadow)

1. All running tasks on this cluster are shown in the "Tasks" section. Click on the running task.
![Tasks](/images/lab5_drift_detection/aws_ecs_task_1.png?width=800px&classes=border,shadow)

1. In the "Containers" section you will see the two containers that run as part of this task.
![Containers](/images/lab5_drift_detection/aws_ecs_containers_1.png?width=800px&classes=border,shadow)

1. On the top of the same page, switch to the "Logs" tab to examine logs coming in from the two containers. If there are no logs shown immediately it could be because the task is pending (not running). Logs start streaming once the task is running.
![Logs](/images/lab5_drift_detection/aws_ecs_logs_1.png?width=800px&classes=border,shadow)

1. You can examine the logs from your "Deploy" job here. Or you could click on "View in CloudWatch" pulldown and select "/ecs/liquibase-flow" to switch to Amazon CloudWatch to look at this log. The reason you see two entries for the same CloudWatch logs (_/ecs/liquibase-flow_) is because the two containers are logging to the same log group. One entry will take you to logs for SQL-Repository container while the other entry will take you to logs for Liquibase-Pro. 
![View in CloudWatch](/images/lab5_drift_detection/aws_ecs_view_in_cloudwatch.png?width=800px&classes=border,shadow)

1. If there are no errors, this task should create a snapshot file in our S3 bucket. 

    1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "S3".  Then click "S3" under Services:
    ![Console home search](/images/lab5_drift_detection/aws_console_home_search_3.png?width=600px&classes=border,shadow)

    1. Click on the S3 bucket __liquibase-workshop-bucket__.
    ![liquibase-workshop-bucket](/images/lab5_drift_detection/aws_s3_bucket_1.png?width=600px&classes=border,shadow)

    1. Notice that there is a __dev-snapshots__ directory. Click this directory.
    ![liquibase-workshop-bucket](/images/lab5_drift_detection/aws_s3_snapshots_1.png?width=600px&classes=border,shadow)

    1. This is your snapshot file. If you like, you can click on the checkbox and click "Open" to view this file.
    ![liquibase-workshop-bucket](/images/lab5_drift_detection/aws_s3_snapshots_2.png?width=600px&classes=border,shadow)

We will use this file in the next lesson to compare the state of the DEV database against it's snapshot.
