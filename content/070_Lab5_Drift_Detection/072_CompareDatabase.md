---
title: "Compare Database" # MODIFY THIS TITLE
chapter: true
weight: 72 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Compare Database

Liquibase allows you to compare two databases or the current database against its saved snapshot using the "__diff__" command. The diff command allows you to detect drift by:

* Finding missing objects between two databases or between the current database and it's saved snapshot
* Seeing that a change was made to your database
* Finding unexpected items in your database

In addition, you can provide arguments for drift severity and for generating an HTML drift report.

Let us configure our flow file to compare our dev database prior to performing other database operations.

![Compare and Snapshot stage](/images/lab5_drift_detection/flow_file_w_snapshot_and_diff.png?width=400px&classes=border,shadow) 


## sqlcode/flow.yaml

We will modify the existing Liquibase Flow file in the GitHub repository, __sqlcode/flow.yaml__. 

Add a __Compare__ stage between the __Connect__ stage and the __Checks__ stage. Your __Compare__ stage should look like this:

```yaml
...
...
...
  Connect:
    actions:
      - type: liquibase
        command: connect

  Compare:
    actions:
      - type: liquibase
        command: diff
        cmdArgs: { 
                    reference-url: "offline:postgresql?snapshot=${BRANCH}-snapshots/mySnapshot.json",
                    format: "json",
                    drift-severity: 1,
                    drift-severity-missing: 2,
                    drift-severity-unexpected: 3,
                    drift-severity-changed: 4
                  }
        globalArgs: { 
                    mirror-console-messages-to-log: "true",
                    reports-enabled: "true",
                    reports-path: "reports",
                    reports-name: "s3://liquibase-workshop-bucket/reports/drift-report-${BRANCH}.html"
                  }       

  Checks:
    actions:
      - type: liquibase
        command: checks show
        cmdArgs: {
                    check-status: enabled,
                    auto-update: off,
                    checks-settings-file: "s3://liquibase-workshop-bucket/liquibase.checks-settings.conf"
            }

```

## task-def-side-car.json

Next, we will modify the __task-def-side-car.json__ file provided in the GitHub repository. We need to add the S3 bucket location in the Liquibase command's __--search-path__ argument. The new argument should look like this: `"--search-path=/sqlcode,s3://liquibase-workshop-bucket"`. 

Your modified __task-def-side-car.json__ file should now look like this (only a subsection of the file shown):

```json
        {
            "name": "Liquibase-Pro",
            "image": "709825985650.dkr.ecr.us-east-1.amazonaws.com/liquibase/liquibase/liquibasepro:4.30.0",
            "cpu": 0,
            "portMappings": [],
            "essential": true,
            "command": [
                "--defaults-file=/sqlcode/liquibase.properties",
                "--search-path=/sqlcode,s3://liquibase-workshop-bucket",
                "--changelog-file=changelog.yaml",
                "flow",
                "--flow-file=/sqlcode/flow.yaml"
            ],
            "environment": [
                {
                  "name": "AWS_REGION","value": "us-west-2"
                },
                {
                  "name": "INSTALL_MYSQL","value": "true"
                }
              ],
            "environmentFiles": [],
            "mountPoints": [
                {
                    "sourceVolume": "sqlrepository",
                    "containerPath": "/sqlcode",
                    "readOnly": false
                }
            ],
            "volumesFrom": [
                {
                    "sourceContainer": "SQL-Repository",
                    "readOnly": false
                }
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "/ecs/liquibase-flow",
                    "awslogs-create-group": "true",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "ecs"
                },
                "secretOptions": []
            },
            "systemControls": []
        }
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
    git commit -m "compare database added"
    git push -u origin dev
    ```

The __git push__ command will automatically trigger a workflow in GitHub Actions.

## Validate that the GitHub Actions workflow ran with the Compare stage 

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

1. If there are no errors, this task should create a drift report file in our S3 bucket. 

    1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "S3".  Then click "S3" under Services:
    ![Console home search](/images/lab5_drift_detection/aws_console_home_search_3.png?width=600px&classes=border,shadow)

    1. Click on the S3 bucket __liquibase-workshop-bucket__.
    ![liquibase-workshop-bucket](/images/lab5_drift_detection/aws_s3_bucket_1.png?width=600px&classes=border,shadow)

    1. Notice that there is a __reports__ directory. Click this directory.
    ![reports](/images/lab5_drift_detection/aws_s3_driftreports_1.png?width=600px&classes=border,shadow)

    1. This is your drift report file. If you like, you can click on the checkbox and click "Open" to view this file.
    ![drift report](/images/lab5_drift_detection/aws_s3_driftreports_2.png?width=600px&classes=border,shadow)
    ![drift report](/images/lab5_drift_detection/aws_s3_driftreports_3.png?width=600px&classes=border,shadow)

Notice that drift report shows that there are no differences. 

If you were to add any object to this database outside of Liquibase automation then this report would show differences.
