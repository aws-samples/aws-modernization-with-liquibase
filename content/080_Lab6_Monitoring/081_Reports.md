---
title: "Operation Reports" # MODIFY THIS TITLE
chapter: true
weight: 81 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Operations Reports

We have actually been generating operations reports each time you ran the deploy or the rollback workflow. This is configured in GitHub Actions workflow files (__aws.yml__ and __aws_rollback.yml__). 

Operations reports are easy to configure. You just need to establish one or more environent variables and Liqubiase Pro will use these environment variables to configure report name and path. 

## Reports from DB Update

The "DB Update" workflow is configured by __aws.yml__ file. This workflow executes two Liquibase commands: "checks run" and "update". 

Here is a section from the __aws.yml__ file which configure environment variables for __checks run__ and __update__ operations report. We give a different name to each report :

1. "checks run" command operation report with __CHECKS-${{ env.TAG }}__.
1. "update" command operation report with __UPDATE-${{ env.TAG }}__. 

Path for these operations report is configured for S3 location:

__s3://liquibase-workshop-bucket/reports/update-${{ env.TAG }}/__

```yaml
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
```

Here is a section from the __aws_rollback.yml__ file which configure environment variables for rollback operation report. 

1. We name the "rollback-one-update" command operation report with __ROLLBACK-${{ env.TAG }}__.

Path for the rollback operation report is configured for the following S3 location:

__s3://liquibase-workshop-bucket/reports/rollback-${{ env.TAG }}/__

```yaml
        environment-variables: |
          LIQUIBASE_COMMAND_URL=${{ vars.LIQUIBASE_COMMAND_URL }}
          LIQUIBASE_COMMAND_USERNAME=${{ vars.LIQUIBASE_COMMAND_USERNAME }}
          LIQUIBASE_COMMAND_PASSWORD=${{ vars.LIQUIBASE_COMMAND_PASSWORD }}
          LIQUIBASE_COMMAND_TAG=${{ env.TAG }}
          LIQUIBASE_REPORTS_PATH=s3://liquibase-workshop-bucket/reports/rollback-${{ env.TAG }}/
          LIQUIBASE_COMMAND_ROLLBACK_ONE_UPDATE_REPORT_NAME=ROLLBACK-${{ env.TAG }}.html
          BRANCH=${{ github.ref_name }}
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
    git commit -m "reports added"
    git push -u origin dev
    ```

The __git push__ command will automatically trigger a workflow in GitHub Actions.

## Validate that the GitHub Actions workflow ran and your reports appear in the S3 bucket

You can browse into S3 and look at these reports:

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "S3". Then click "S3" under Services.
     ![Console home](/images/lab6_monitoring/aws_console_home_1.png?width=600px&classes=border,shadow)

1. Click into the "liquibase-workshop-bucket".
     ![liquibase-workshop-bucket](/images/lab6_monitoring/s3_liquibase_workshop_bucket.png?width=600px&classes=border,shadow)

1. You will see the "reports" folder. Click into this folder.
     ![reports](/images/lab6_monitoring/s3_reports_0.png?width=400px&classes=border,shadow)

1. You will see a folder for each workflow you ran.
     ![reports](/images/lab6_monitoring/s3_reports_1.png?width=400px&classes=border,shadow)

1. Click into the "__update__" folder to look at the "UPDATE" and "CHECKS" html reports. Click into the "__rollback__" folder to look at the "ROLLBACK" html report.
     ![update reports](/images/lab6_monitoring/s3_reports_2.png?width=400px&classes=border,shadow)
     ![rollback reports](/images/lab6_monitoring/s3_reports_3.png?width=400px&classes=border,shadow)

1. You can click on any report. You can download and open the report in your browser.
     ![click report](/images/lab6_monitoring/s3_reports_4.png?width=800px&classes=border,shadow)
     ![download or open report](/images/lab6_monitoring/s3_reports_5.png?width=800px&classes=border,shadow)


