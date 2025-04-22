---
title: "Setup AWS ECS" # MODIFY THIS TITLE
chapter: true
weight: 25 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Setup Elastic Container Service

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

In this section you will create a new cluster and a new task definition.

## Create New Cluster

Let's create a new cluster. These are a few steps to complete:

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "ecs".  Then click "Elastic Container Services" under Services:

    ![Console home search](/images/self_guided_setup/aws_ecs_1.png?width=600px&classes=border,shadow)

1. Create a new cluster. Click "Create cluster".

    ![Console home search](/images/self_guided_setup/aws_ecs_cluster_create.png?width=800px&classes=border,shadow)

1. Give the cluster a name, e.g., `liquibase-cluster`.

    ![Console home search](/images/self_guided_setup/aws_ecs_cluster_name.png?width=600px&classes=border,shadow)

1. In Infrastructure section, select "AWS Fargate (serverless)" and click "Create".

    ![AWS Fargate](/images/self_guided_setup/aws_ecs_infrastructure.png?width=600px&classes=border,shadow)

The new cluster is now ready for your use. Tasks running on this cluster will use AWS Fargate serverless infrastructure.

    ![Cluster Ready](/images/self_guided_setup/aws_ecs_cluster_ready.png?width=800px&classes=border,shadow)

## Create New Task Definition

Let's create a new task definition. These are a few steps to complete:

1. Click "Task definitions" in the left pane and then click "Create new task definition" > "Create new task definition".

    ![Create New Task Definition](/images/self_guided_setup/aws_ecs_task_defintion_create.png?width=1000px&classes=border,shadow)

1. Give a name for "Task definition family", e.g., `liquibase-flow`.

    ![Task Definition Family](/images/self_guided_setup/aws_ecs_task_definition_family.png?width=800px&classes=border,shadow)

1. Configure "Infrastructure requirements".
    * Choose "AWS Fargate".
    * Choose "Operation system/architecture": `Linux/X86_64`.
    * Select "Task role" and "Task execute role" as `ecsTaskExecutionRole` you configured previously in [Configure “ecsTaskExecutionRole” Role](../020_self_guided_setup/021_setupawsaccount.html)

    ![Task Definition Family](/images/self_guided_setup/aws_ecs_task_definition_infra_requirements.png?width=800px&classes=border,shadow)

1. Configure "Container-1".
    * Name: `Liquibase-Pro`
    * Image URI: `709825985650.dkr.ecr.us-east-1.amazonaws.com/liquibase/liquibase/liquibasepro:4.29.2-latest`
    * Essential container: Yes
    * Click "Remove" to remove any port mappings.

    ![Container-1](/images/self_guided_setup/aws_ecs_task_definition_cointainer1_name.png?width=800px&classes=border,shadow)
    ![Container-1](/images/self_guided_setup/aws_ecs_task_definition_cpu.png?width=800px&classes=border,shadow)

1. Add two environment variables:
    * AWS_REGION: _\<the region configured for the user\>_
    * INSTALL_MYSQL: true/false _("true" if you are connecting to a MySQL database)_

    ![Environment Variables](/images/self_guided_setup/aws_ecs_task_definition_env_vars.png?width=800px&classes=border,shadow)

1. In the "Log collection" section, remove these two items:
    * mode
    * max-buffer-size

    ![Log Collection](/images/self_guided_setup/aws_ecs_task_definition_log_collection.png?width=800px&classes=border,shadow)

1. Scroll down and expand the "Docker configuration" section. In the "Command" field, type: 
    ```
    --defaults-file=/sqlcode/liquibase.properties,--search-path=/sqlcode,--changelog-file=changelog.yaml,flow,--flow-file=/sqlcode/flow.yaml
    ```

    ![Docker Configuration](/images/self_guided_setup/aws_ecs_task_definition_docker_configuration_1.png?width=800px&classes=border,shadow)
    ![Docker Command](/images/self_guided_setup/aws_ecs_task_definition_docker_configuration_2.png?width=800px&classes=border,shadow)

1. Scroll down and click "Create".

    ![Create Task Definition](/images/self_guided_setup/aws_ecs_task_definition_done.png?width=800px&classes=border,shadow)

1. You should see your task definition setup as follows. AWS ECS stores each version of task definition. So the first time you create your task definition it will be versioned as __liquibase-flow:1__. Each subsequent update to this task definition will get a new version. 

    ![Create Task Definition](/images/self_guided_setup/aws_ecs_task_definition_ready.png?width=800px&classes=border,shadow)

