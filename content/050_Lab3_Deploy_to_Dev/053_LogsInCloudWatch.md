---
title: "Logs in AWS CloudWatch" # MODIFY THIS TITLE
chapter: true
weight: 53 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Examine Logs in AWS CloudWatch

Recall that we configured our task definition file (_task-def-sidecar.json_) to send all logs to _"/ecs/liquibase-flow"_ log group in CloudWatch. As a result, all logs from AWS ECS tasks are recorded here.

Sample logs for SQL-Repository container:
![SQL-Repository logs](/images/lab3_deploy_to_dev/aws_cloudwatch_sql-repository.png?width=800px&classes=border,shadow)

Sample logs for Liquibase-Pro container:
![SQL-Repository logs](/images/lab3_deploy_to_dev/aws_cloudwatch_liquibase-pro.png?width=800px&classes=border,shadow)


