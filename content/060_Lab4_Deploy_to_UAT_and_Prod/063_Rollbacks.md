---
title: "Rollbacks" # MODIFY THIS TITLE
chapter: true
weight: 63 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Database Rollback by Liquibase

In our example repository, we have provided rollback scripts. For example, have a look in the `sqlcode/database1/tables` directory. Each script has a corresponding rollback script.  

```log
% tree sqlcode/database1/tables
sqlcode/database1/tables
├── actor-rollback.sql
├── actor.sql
├── address-rollback.sql
├── address.sql
├── category-rollback.sql
├── category.sql
├── changelog.yaml
├── city-rollback.sql
├── city.sql
├── country-rollback.sql
├── country.sql
├── customer-rollback.sql
├── customer.sql
├── film-rollback.sql
├── film.sql
├── film_actor-rollback.sql
├── film_actor.sql
├── film_category-rollback.sql
├── film_category.sql
├── inventory-rollback.sql
├── inventory.sql
├── language-rollback.sql
├── language.sql
├── payment-rollback.sql
├── payment.sql
├── rental-rollback.sql
├── rental.sql
├── staff-rollback.sql
├── staff.sql
├── store-rollback.sql
└── store.sql
```

The "changelog.yaml" file in this directory shows how each changeset pairs one script with a corresponding rollback script. 

 ![Changeset rollback](/images/lab4_deploy_to_uat_prod/changeset_rollback_1.png?width=600px&classes=border,shadow)


Let us look at rolling back our changes with Liquibase. 

Liquibase provides several commands to perform rollbacks. We will be using the `rollback-one-update` command to rollback all changes that were deployed in a single update operation in Liquibase. 

In the sample repository we have provided files for automating database rollback.
* __.github/workflows/aws_rollback.yml__: This is a Github Actions workflow file to execute the rollback workflow.
* __task-def-sidecar-rollback.json__: A rollback version of the original "task-def-sidecar.json". We still create a new image for SQL repository. However, we execute a different flow file with the rollback command.
* __sqlcode/flow-rollback.yaml__: This is the flow file with the rollback command ("rollback-one-update"). 

Follow these steps to configure a rollback workflow in GitHub Actions.

1. Let's switch back to our "dev" branch and fetch latest changes. In your command line terminal, run these commands:
```shell
git checkout dev
git fetch origin
```

1. Create a GitHub Actions variable "MY_ECS_TASK_DEFINITION_ROLLBACK" and set it's value to "task-def-sidecar-rollback.json".
     1. In Github's browser page, click on "Settings" tab.
     2. Expand "Secrets and Variables" in the left pane.
     3. Click the "Actions" option. 
     4. Switch to the "Variables" tab.
     5. Click the "New repository variable" button.
     ![New repository variable](/images/lab4_deploy_to_uat_prod/github_variable_0.png?width=600px&classes=border,shadow)
     ![MY_ECS_TASK_DEFINITION_ROLLBACK](/images/lab4_deploy_to_uat_prod/github_variable_1.png?width=600px&classes=border,shadow)

1. Rollback UAT environment. 
     1. Switch to the "Actions" tab. 
     2. Then click on "DB Rollback" workflow in the left pane. 
     3. Click the "Run workflow" button/drop-down.
     4. Select "uat" branch.
     5. Click the "Run workflow" button.
     ![Rollback DEV](/images/lab4_deploy_to_uat_prod/github_actions_rollback_dev_0.png?width=800px&classes=border,shadow)

1. It takes a few seconds for the new rollback workflow to show up on the screen. 
     ![GitHub actions](/images/lab4_deploy_to_uat_prod/github_actions_rollback_dev_2.png?width=800px&classes=border,shadow)

1. You can click into this workflow to access the rollback job.
     ![GitHub actions](/images/lab4_deploy_to_uat_prod/github_actions_rollback_dev_3.png?width=800px&classes=border,shadow)

1. This workflow will start a task in Amazon ECS running the "flow-rollback.yaml" flow file. Since we selected the "uat" branch, this will rollback changes from the database in the DEV environment.

### Validate that changes are rolled back from the RDS Aurora PostgreSQL database in the DEV environment

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "ECS". Then click "Elastic Container Service" under Services:
![Console home](/images/lab4_deploy_to_uat_prod/aws_console_home_1.png?width=600px&classes=border,shadow)

1. Click on "Clusters" in the left pane. Notice that if there is a task running on this cluster it will indicate as _pending_ or _running_. Click "workshop-cluster" to get to the details of this running task.
![AWS ECS Cluster](/images/lab4_deploy_to_uat_prod/aws_ecs_cluster_1.png?width=800px&classes=border,shadow)

1. All running tasks on this cluster are shown in the "Tasks" section. Click on the running task.
![Tasks](/images/lab4_deploy_to_uat_prod/aws_ecs_task_1.png?width=800px&classes=border,shadow)

1. In the "Containers" section you will see the two containers that run as part of this task.
![Containers](/images/lab4_deploy_to_uat_prod/aws_ecs_containers_1.png?width=800px&classes=border,shadow)

1. On the top of the same page, switch to the "Logs" tab to examine logs coming in from the two containers. If there are no logs shown immediately it could be because the task is pending (not running). Logs start streaming once the task is running.
![Logs](/images/lab4_deploy_to_uat_prod/aws_ecs_logs_1.png?width=800px&classes=border,shadow)

You can examine the logs from your "Rollback" job here. Or you could click on "View in CloudWatch" pulldown and select "/ecs/liquibase-flow" to switch to Amazon CloudWatch to look at this log. The reason you see two entries for the same CloudWatch logs (_/ecs/liquibase-flow_) is because the two containers are logging to the same log group. One entry will take you to logs for SQL-Repository container while the other entry will take you to logs for Liquibase-Pro. 
![View in CloudWatch](/images/lab4_deploy_to_uat_prod/aws_ecs_view_in_cloudwatch.png?width=800px&classes=border,shadow)

