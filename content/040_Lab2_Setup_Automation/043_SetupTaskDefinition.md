---
title: "Setup Task Definition" # MODIFY THIS TITLE
chapter: true
weight: 43 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setup Task Definition File for AWS ECS

This is where we will configure the two Docker images as a single task definition.

## Task Definition File

A task definition file is a JSON-formatted text file that describes the parameters and containers that make up an application running on Amazon Elastic Container Service (ECS).

If you recall, in [Setup AWS ECS page](../020_self_guided_setup/025_setupawsecs.html), we configured a task definition called __liquibase_flow__. This task definition was configured with a single image, the Liquibase Pro image, from AWS Market Place. This task definition can be exported into a JSON file. 

We will now expand this task definition to include the second image to enable deploying SQL scripts from the GitHub repository.

### task-def-sidecar.json

A completed task definition file, __task-def-sidecar.json__, has been provided in the GitHub repository. 

![task-def-sidecar.json](/images/lab2_setup_automation/aws_ecs_task_definition_2.png?width=800px&classes=border,shadow) 

Let's examine it's content:

1. __containerDefinitions__ - There are two containerDefinitions in this JSON file corresponding to SQL-Repository and Liquibase-Pro images.
1. __SQL-Repository__ container definition - 
    1. __mountPoints__ - mounts /sqlcode directory in the container to __sqlrepository__ source volume.
    1. __logConfiguration__ - sets up logging so that all logs are sent to __/ecs/liquibase-flow__ log group.
    1. __environment__ - define any environment variables here needed to run this container. 
    1. __command__ - Runs __alive.sh__ 
1. __Liquibase-Pro__ container definition - 
    1. __mountPoints__ - mounts /sqlcode directory in the container to __sqlrepository__ source volume.
    1. __logConfiguration__ - sets up logging so that all logs are sent to __/ecs/liquibase-flow__ log group.
    1. __environment__ - define any environment variables here needed to run this container.
    1. __volumesFrom__ - Indicate that the source of the volume is from __SQL-Repository__ container.
    1. __command__ - Runs __liquibase__ with various arguments.


```json
{
    "family": "liquibase-flow",
    "containerDefinitions": [
        {
            "name": "SQL-Repository",
            "image": "442012004811.dkr.ecr.us-west-2.amazonaws.com/liquibase/sqlrepository",
            "cpu": 0,
            "portMappings": [],
            "command": [
                "/bin/sh",
                "./alive.sh"
            ],
            "essential": false,
            "environment": [
                {
                    "name": "AWS_DEFAULT_REGION",
                    "value": "us-west-2"
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
            "volumesFrom": [],
            "systemControls": []
        },
        {
            "name": "Liquibase-Pro",
            "image": "709825985650.dkr.ecr.us-east-1.amazonaws.com/liquibase/liquibase/liquibasepro:4.30.0-latest",
            "cpu": 0,
            "portMappings": [],
            "essential": true,
            "command": [
                "--defaults-file=/sqlcode/liquibase.properties",
                "--search-path=/sqlcode",
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
    ],
    "taskRoleArn": "arn:aws:iam::442012004811:role/ecsTaskExecutionRole",
    "executionRoleArn": "arn:aws:iam::442012004811:role/ecsTaskExecutionRole",
    "networkMode": "awsvpc",
    "volumes": [
        {
            "name": "sqlrepository",
            "host": {}
        }
    ],
    "placementConstraints": [],
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "1024",
    "memory": "2048",
    "runtimePlatform": {
        "cpuArchitecture": "X86_64",
        "operatingSystemFamily": "LINUX"
    }
}
```



