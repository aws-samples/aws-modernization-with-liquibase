---
title: "Setup GitHub Actions" # MODIFY THIS TITLE
chapter: true
weight: 42 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setup GitHub Actions

In this section, we will setup GitHub Actions as part of this GitHub repository. GitHub Actions are configured using a YAML workflow file.

An __aws.yaml__ workflow file has been provided in the __.github/workflows directory__.
![aws.yaml](/images/lab2_setup_automation/github_aws_yaml_1.png?width=800px&classes=border,shadow)

Let's examine the organization of this workflow:
1. __name:__ - The name of the GitHub Actions workflow
1. __on:__ - Indicates when this workflow is triggered
1. __env:__ - Environment variables to be used in the workflow
1. __jobs:__ - Specify jobs. The only job configured in this workflow is the job name "Deploy to dev". 
    * The job has an _if_ condition to only run when the branch name is 'dev'. 
1. __steps:__ - Various steps that perform action or actions. 

Key steps in this workflow file are:

{{% notice note %}}
You will need to configure this file with network configuration specific to your AWS account. You will need to modify __run-task-security-groups__ and __run-task-subnets__ in these steps below.
{{% /notice %}}

1. __Configure AWS credentials__ - This step uses aws-actions to configure user based on access key id, secret access key and region.
1. __Login to Amazon ECR__ - Performs a login to Amazon ECR in preparation for the next step to push a docker image to the ECR repository.
1. __Build, tag and push image to Amazon ECR__ - This is where the SQL-Repository docker image is created, tagged and pushed to ECR repository.
1. __Fill in the new image ID in the Amazon ECS task definition__ - The task definition is updated with the information about the new SQL-Repository image.
1. __Deploy Amazon ECS task definition__ - Deploy the task definition, using "run task" option.
    1. __run-task:__ - _true_
    1. __run-task-subnets:__ - :exclamation: Provide information about subnets (change to your subnets)
    1. __run-task-security-groups:__ - :exclamation: Provide the ID of the security group (change to your security groups)
    1. __run-task-assign-public-IP:__ - _ENABLED_
```yaml
name: DB Update

on:
  workflow_dispatch:
    branches: [ "dev" ]
  push:
    branches: [ "dev" ]

env:
  AWS_ACCOUNT_NUMBER: ${{ secrets.AWS_ACCOUNT_NUMBER }}
  AWS_REGION: ${{ secrets.AWS_REGION }}   # set this to your preferred AWS region, e.g. us-west-1
  ECR_REPOSITORY: liquibase/sqlrepository # set this to your Amazon ECR repository name
  ECS_CLUSTER: ${{ vars.MY_ECS_CLUSTER }}  # set this to your Amazon ECS cluster name
  ECS_TASK_DEFINITION: task-def-sidecar.json # set this to the path to your Amazon ECS task definition
  CONTAINER_NAME: ${{ vars.MY_CONTAINER_NAME }} # set this to the name of the container in the
  IMAGE_NAME: ${{ vars.IMAGE_NAME }}

permissions:
  contents: read

jobs:
  deploy:
    name: Deploy to dev
    runs-on: ubuntu-latest
    environment: ${{ github.ref_name }}
    if: github.ref_name == 'dev' || github.ref_name == 'uat' || github.ref_name == 'main' 

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: Build, tag, and push image to Amazon ECR
      id: build-image
      env:
        ECR_REGISTRY: ${{ env.AWS_ACCOUNT_NUMBER }}.dkr.ecr.us-west-2.amazonaws.com
        IMAGE_TAG: ${{ github.sha }}
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .        
        docker tag $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG $ECR_REGISTRY/$ECR_REPOSITORY:latest
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:latest
        echo "image=$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG" >> $GITHUB_OUTPUT

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
          LIQUIBASE_REPORTS_PATH=s3://liquibase-workshop-bucket/reports/${{ env.TAG }}/
          LIQUIBASE_COMMAND_CHECKS_RUN_REPORT_NAME=CHECKS-${{ env.TAG }}.html
          LIQUIBASE_COMMAND_UPDATE_REPORT_NAME=UPDATE-${{ env.TAG }}.html

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

:exclamation: Be sure to provide your own subnets and security groups ID. 

Then __save__ the workflow file and, using a terminal window, commit to the GitHub repository.

```shell
git add .
git commit -m "updated workflow"
git push
```