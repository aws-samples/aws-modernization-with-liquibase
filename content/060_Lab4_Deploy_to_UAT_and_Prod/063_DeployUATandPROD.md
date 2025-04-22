---
title: "Deploy to UAT and PROD" # MODIFY THIS TITLE
chapter: true
weight: 63 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Deploy to UAT and PROD databases

In order to deploy to UAT and PROD database, we will modify our __aws.yaml__ file. 

* We will deploy to UAT database when changes are merged into the "uat" branch.
* We will deploy to PROD database when change are merged into the "main" branch.

## Deploy to RDS Aurora PostgreSQL database in the UAT environment

In order to deploy to the UAT environment, we need to make a few changes to our automation:

1. Update aws.yaml to trigger when changes are merged into the __uat__ branch.
1. Merge repository changes into the __uat__ branch.
1. Validate that changes are deployed to the RDS Aurora PostgreSQL database in the UAT environment

### Update "aws.yaml"


In your __aws.yaml__ file, make the following changes:

1. In the "push to branches" section, add __uat__ and __main__ branches.
    ![On Push Branches](/images/lab4_deploy_to_uat_prod/aws_yaml_1.png?width=400px&classes=border,shadow)

1. In the "jobs deploy" section:
    * Change __dev__ to `${{ github.ref_name }}`.
    * Change the __if: github.ref_name == 'dev'__ condition to include conditional check for additional branches `if: github.ref_name == 'dev' || github.ref_name == 'uat' || github.ref_name == 'main'`
    ![Deploy job](/images/lab4_deploy_to_uat_prod/aws_yaml_2.png?width=800px&classes=border,shadow)


### Merge repository changes into the "uat" branch

1. In your command line terminal, run the following commands to create a new "uat" branch and then switch to using this branch:
    ```shell
    git branch uat
    git checkout uat
    ```

1. Run `git branch` command to ensure that you are on the "uat" branch.
    ![uat branch](/images/lab4_deploy_to_uat_prod/git_branch_uat.png?width=400px&classes=border,shadow)


1. Fetch the latest changes from the remote repository. Since this is the new branch there is nothing to fetch but it is a good practice to always run this command:
    ```shell
    git fetch origin
    ```

1. Merge the source branch ("dev") to the "uat" branch.
    ```shell
    git merge dev
    ```

1. Add, commit and push changes to the "uat" branch.
    ```shell
    git add .
    git commit -m "first uat merge"
    git push -u origin uat
```

1. This automatically triggers a GitHub Actions workflow.
    ![Github Actions](/images/lab4_deploy_to_uat_prod/github_actions_1.png?width=800px&classes=border,shadow)

1. You can click into this workflow to access the deploy job.
    ![Deploy to uat](/images/lab4_deploy_to_uat_prod/github_actions_deploy_to_uat.png?width=800px&classes=border,shadow)


### Validate that changes are deployed to the RDS Aurora PostgreSQL database in UAT

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

You can examine the logs from your "Deploy" job here. Or you could click on "View in CloudWatch" pulldown and select "/ecs/liquibase-flow" to switch to Amazon CloudWatch to look at this log. The reason you see two entries for the same CloudWatch logs (_/ecs/liquibase-flow_) is because the two containers are logging to the same log group. One entry will take you to logs for SQL-Repository container while the other entry will take you to logs for Liquibase-Pro. 
![View in CloudWatch](/images/lab4_deploy_to_uat_prod/aws_ecs_view_in_cloudwatch.png?width=800px&classes=border,shadow)



## Deploy to RDS Aurora PostgreSQL database in PROD

1. Merge repository changes into the __prod__ branch.
1. Validate that changes are deployed to the RDS Aurora PostgreSQL database in the PROD environment

### Merge repository changes into the "prod" branch

1. In your command line terminal, run the following commands to create a new "uat" branch and then switch to using this branch:
```shell
git branch main
git checkout main
```

1. Run `git branch` command to ensure that you are on the "main" branch.
    ![main branch](/images/lab4_deploy_to_uat_prod/git_branch_main.png?width=400px&classes=border,shadow)


1. Fetch the latest changes from the remote repository. Since this is the new branch there is nothing to fetch but it is a good practice to always run this command:
```shell
git fetch origin
```

1. Merge the source branch ("dev") to the "uat" branch.
```shell
git merge uat
```

1. Add, commit and push changes to the "uat" branch.
```shell
git add .
git commit -m "first prod merge"
git push -u origin main
```

1. This automatically triggers a GitHub Actions workflow.
    ![Github Actions](/images/lab4_deploy_to_uat_prod/github_actions_1.png?width=800px&classes=border,shadow)

1. You can click into this workflow to access the deploy job.
    ![Deploy to main](/images/lab4_deploy_to_uat_prod/github_actions_deploy_to_main.png?width=800px&classes=border,shadow)


### Validate that changes are deployed to the RDS Aurora PostgreSQL database in the PROD environment

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

You can examine the logs from your "Deploy" job here. Or you could click on "View in CloudWatch" pulldown and select "/ecs/liquibase-flow" to switch to Amazon CloudWatch to look at this log. The reason you see two entries for the same CloudWatch logs (_/ecs/liquibase-flow_) is because the two containers are logging to the same log group. One entry will take you to logs for SQL-Repository container while the other entry will take you to logs for Liquibase-Pro. 
![View in CloudWatch](/images/lab4_deploy_to_uat_prod/aws_ecs_view_in_cloudwatch.png?width=800px&classes=border,shadow)
