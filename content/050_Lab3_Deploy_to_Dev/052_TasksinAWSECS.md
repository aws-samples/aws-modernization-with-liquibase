---
title: "Tasks in AWS ECS" # MODIFY THIS TITLE
chapter: true
weight: 52 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Tasks in AWS ECS

Tasks running in AWS ECS execute on a cluster we previously configured called "workshop-cluster".

Note that the execution may only last less than 1 minute. If you do not see a task running in the cluster it is possible that it has completed. 

These are a few steps to complete:

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "ECS". Then click "Elastic Container Service" under Services:
![Console home](/images/lab3_deploy_to_dev/aws_console_home_1.png?width=600px&classes=border,shadow)

1. Click on "Clusters" in the left pane. Notice that if there is a task running on this cluster it will indicate as _pending_ or _running_. Click "workshop-cluster" to get to the details of this running task.
![AWS ECS Cluster](/images/lab3_deploy_to_dev/aws_ecs_cluster_1.png?width=800px&classes=border,shadow)

1. All running tasks on this cluster are shown in the "Tasks" section. Click on the running task.
![Tasks](/images/lab3_deploy_to_dev/aws_ecs_task_1.png?width=800px&classes=border,shadow)

1. In the "Containers" section you will see the two containers that run as part of this task.
![Containers](/images/lab3_deploy_to_dev/aws_ecs_containers_1.png?width=800px&classes=border,shadow)

1. On the top of the same page, switch to the "Logs" tab to examine logs coming in from the two containers. If there are no logs shown immediately it could be because the task is pending (not running). Logs start streaming once the task is running.
![Logs](/images/lab3_deploy_to_dev/aws_ecs_logs_1.png?width=800px&classes=border,shadow)

You can examine the logs from your "Deploy" job here. Or you could click on "View in CloudWatch" pulldown and select "/ecs/liquibase-flow" to switch to Amazon CloudWatch to look at this log. The reason you see two entries for the same CloudWatch logs (_/ecs/liquibase-flow_) is because the two containers are logging to the same log group. One entry will take you to logs for SQL-Repository container while the other entry will take you to logs for Liquibase-Pro. 
![View in CloudWatch](/images/lab3_deploy_to_dev/aws_ecs_view_in_cloudwatch.png?width=800px&classes=border,shadow)


