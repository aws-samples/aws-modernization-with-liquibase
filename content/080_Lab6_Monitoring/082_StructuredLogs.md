---
title: "Structured Logs" # MODIFY THIS TITLE
chapter: true
weight: 82 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Structured Logging

Liquibase allows you to put logs into a machine-readable format such that you can use observability tools, such as AWS CloudWatch, Grafana, Splunk, ElasticSearch and many others, for monitoring and analysis to easily determine and act upon both real-time and long term trend data. 

Liquibase can generate structured logs in a JSON format.

## Enable Structured Logging

Structured logging can be enabled by:
1. Setting the __LIQUIBASE_LOG_FORMAT__ environment variable to JSON. This can be set to JSON or JSON_PRETTY.
2. Setting the __LIQUIBASE_LOG_LEVEL__ environment variable to at least INFO. This can be set to INFO or FINE.

Both the __aws.yml__ and __aws_rollback.yml__ files requires updating to configure an environment variable for structured logging. 

Here is a section from the __aws.yml__ file which configure environment variables for structured logging during the database update operation.

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
          LIQUIBASE_LOG_FORMAT=JSON
          LIQUIBASE_LOG_LEVEL=INFO
          BRANCH=${{ github.ref_name }}
          
    - name: Deploy Amazon ECS task definition
      uses: aws-actions/amazon-ecs-deploy-task-definition@v2
      with:
        # task-definition: ${{ env.ECS_TASK_DEFINITION }}
        task-definition: ${{ steps.render-task-def.outputs.task-definition }}
        cluster: ${{ env.ECS_CLUSTER }}
        run-task: true
        run-task-subnets: subnet-bf7d7ada,subnet-345e801f,subnet-0acebc53,subnet-13624764
        run-task-security-groups: sg-215c6345
        run-task-assign-public-IP: ENABLED
```

Here is a section from the __aws_rollback.yml__ file which configure environment variables for structured logging during the database rollback operation. 

```yaml
        environment-variables: |
          LIQUIBASE_COMMAND_URL=${{ vars.LIQUIBASE_COMMAND_URL }}
          LIQUIBASE_COMMAND_USERNAME=${{ vars.LIQUIBASE_COMMAND_USERNAME }}
          LIQUIBASE_COMMAND_PASSWORD=${{ vars.LIQUIBASE_COMMAND_PASSWORD }}
          LIQUIBASE_COMMAND_TAG=${{ env.TAG }}
          LIQUIBASE_REPORTS_PATH=s3://liquibase-workshop-bucket/reports/rollback-${{ env.TAG }}/
          LIQUIBASE_COMMAND_ROLLBACK_ONE_UPDATE_REPORT_NAME=ROLLBACK-${{ env.TAG }}.html
          LIQUIBASE_LOG_FORMAT=JSON
          LIQUIBASE_LOG_LEVEL=INFO
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
    git commit -m "structured data added"
    git push -u origin dev
    ```

The __git push__ command will automatically trigger a workflow in GitHub Actions.

## Validate that the GitHub Actions workflow ran and your reports appear in the S3 bucket

Browse into Amazon Elastic Container Service (ECS) and observe at these structured logs:

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "ECS". Then click "Elastic Container Service" under Services:
![Console home](/images/lab6_monitoring/aws_console_home_1.png?width=600px&classes=border,shadow)

1. Click on "Clusters" in the left pane. Notice that if there is a task running on this cluster it will indicate as _pending_ or _running_. Click "workshop-cluster" to get to the details of this running task.
![AWS ECS Cluster](/images/lab6_monitoring/aws_ecs_cluster_1.png?width=800px&classes=border,shadow)

1. All running tasks on this cluster are shown in the "Tasks" section. Click on the running task.
![Tasks](/images/lab6_monitoring/aws_ecs_task_1.png?width=800px&classes=border,shadow)

1. In the "Containers" section you will see the two containers that run as part of this task.
![Containers](/images/lab6_monitoring/aws_ecs_containers_1.png?width=800px&classes=border,shadow)

1. On the top of the same page, switch to the "Logs" tab to examine logs coming in from the two containers. If there are no logs shown immediately it could be because the task is pending (not running). Logs start streaming once the task is running.
![Logs](/images/lab6_monitoring/aws_ecs_logs_1.png?width=800px&classes=border,shadow)

1. When you switch to the Liquibase-Pro container logs, you will see your structed logs in JSON format:
![Structured Logs](/images/lab6_monitoring/aws_ecs_logs_2.png?width=800px&classes=border,shadow)
![JSON Logs](/images/lab6_monitoring/aws_ecs_logs_3.png?width=800px&classes=border,shadow)


## Query Structured Data in CloudWatch

You can examine the logs from your "Deploy" job here. However, we will not jump into CloudWatch to view our logs and run queries against them.

1. Click on "View in CloudWatch" pulldown and select "/ecs/liquibase-flow" to switch to Amazon CloudWatch to look at this log.
![View in CloudWatch](/images/lab6_monitoring/aws_ecs_view_in_cloudwatch.png?width=800px&classes=border,shadow)

1. Click on "Logs Insight".
![CloudWatch](/images/lab6_monitoring/aws_cloudwatch_1.png?width=600px&classes=border,shadow)
![CloudWatch Log Insight](/images/lab6_monitoring/aws_cloudwatch_logs_insights_1.png?width=800px&classes=border,shadow)

1. Click in the "Selection criteria" pull down and select the selection for "/ecs/liquibase-flow". 
![Selection criteria](/images/lab6_monitoring/aws_cloudwatch_logs_insights_2.png?width=800px&classes=border,shadow)

1. Logs Insights provides a default query as follows. Run this query by clicking "Run query" button. 
![Default query](/images/lab6_monitoring/aws_cloudwatch_default_query_1.png?width=600px&classes=border,shadow)
![Run query](/images/lab6_monitoring/aws_cloudwatch_run_query_1.png?width=800px&classes=border,shadow)


1. This should produce structured log records matching the default query. If no data is returned you may need to change the timespan.
![Run query](/images/lab6_monitoring/aws_cloudwatch_run_query_2.png?width=800px&classes=border,shadow)

### Custom Query - All Update and Rollback Operations

Let's build a query to find all update and rollback operations that succeeded. Here is what that query could look like:

```
fields @timestamp, @message, changesetsRolledback.changesetCount, changesetsUpdated.changesetCount
| filter liquibaseInternalCommand like /[update|rollbackOneUpdate]/
| filter deploymentOutcome like /success/
| sort by @timestamp desc
```
Run this query in CloudWatch. You may need to expand timespan. If you have previously deployed and/or rolled back database changes successfully then you should see some results. 

Note that this will also show operations which results in no new net changes deployed or rolled back when the database was already up-to-date.
![Custom query](/images/lab6_monitoring/aws_cloudwatch_run_query_3.png?width=800px&classes=border,shadow)

### Custom Query - Unique Environments

Let's update the query to find records for unique deployments to the "dev" database.

```fields @timestamp, @message, liquibaseTargetUrl
| filter liquibaseInternalCommand like /[update|rollbackOneUpdate]/
| filter deploymentOutcome like /success/
| filter liquibaseTargetUrl like 'dev'
| dedup liquibaseTargetUrl
```
![Custom query](/images/lab6_monitoring/aws_cloudwatch_run_query_4.png?width=800px&classes=border,shadow)
