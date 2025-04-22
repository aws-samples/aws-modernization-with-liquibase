---
title: "Setup GitHub Variables" # MODIFY THIS TITLE
chapter: true
weight: 32 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setup GitHub Variables 

In this section, you will complete the following tasks:

* Create repository secrets
* Create environment variables
* Create repository variables

## Configure Repository Secrets

Follow the steps below to setup GitHub repository secrets which will enable GitHub to authenticate IAM user.

1. In your browser window, go to your Github repository. 

    * Go to "Settings"

    ![GitHub repository settings](/images/lab1_setup_liquibase/github_settings_1.png?width=800px&classes=border,shadow)

    * On the left pane, click "Secrets and variables". Then click "Actions".

    ![GitHub secrets](/images/lab1_setup_liquibase/github_secrets_1.png?width=400px&classes=border,shadow)

1. In the "Repository secrets" section, click "New repository secret" button. Create these four secrets, one at a time.

    * __AWS_REGION__ (this is the AWS region where you have setup AWS infrastructure)
    * __AWS_ACCESS_KEY_ID__ (this was in a CSV file in [Configure “workshop” User With Programmatic Access](../020_self_guided_setup/021_setupawsaccount.html)) 
    * __AWS_SECRET_ACCESS_KEY__ (this was saved in a CSV file in [Configure “workshop” User With Programmatic Access](../020_self_guided_setup/021_setupawsaccount.html))
    * __AWS_ACCOUNT_NUMBER__ (this is your 12-digit AWS account number in the top right corner of your AWS pages in the browser)

    ![GitHub repository secrets](/images/lab1_setup_liquibase/github_secrets_2.png?width=800px&classes=border,shadow)
    ![GitHub repository secrets](/images/lab1_setup_liquibase/github_secrets_3.png?width=600px&classes=border,shadow)
    ![GitHub repository secrets](/images/lab1_setup_liquibase/github_secrets_4.png?width=600px&classes=border,shadow)

## Conigure Environments and Variables

We will create the "dev" environment with three variables. We will create additional environments later in the workshop.

1. Click "Environment" in the left pane. Then click "New environments" button.

    ![GitHub environments](/images/lab1_setup_liquibase/github_environments_1.png?width=800px&classes=border,shadow)

1. Our first environment will be called "dev". Type __dev__ in the Name field and click "Configure environment".

    ![Dev](/images/lab1_setup_liquibase/github_environments_dev_1.png?width=800px&classes=border,shadow)

1. Scroll down to the __Environment variables__ section and click "Add environment variable" button.

    ![Add environment variable](/images/lab1_setup_liquibase/github_environments_dev_2.png?width=600px&classes=border,shadow)

1. Create the following variables, one at a time. 
    * Note that Liquibase environment variables referring to secrets in AWS Secrets Manager will use three-part name as follows: aws-secrets,_secretName_,_key_

    * __LIQUIBASE_COMMAND_URL__ : Set this to `aws-secrets,liquibase-workshop-secret,dev_url`.
        ![LIQUIBASE_COMMAND_URL](/images/lab1_setup_liquibase/github_environment_var_1.png?width=600px&classes=border,shadow)
    * __LIQUIBASE_COMMAND_USERNAME__ : Click the "Add environment variable" button.
        ![Add environment variable](/images/lab1_setup_liquibase/github_environment_var_2.png?width=600px&classes=border,shadow)
        * Set this in the form of `aws-secrets,rds!cluster-AAAAA-BBB-CCCC-DDDD-ZZZZ,username`.
        * The string "rds!cluster-AAAAA-BBB-CCCC-DDDD-ZZZZ" should be replaced by the secret from your AWS Secret Manager storing username and password for your RDS database.
        ![AWS Secrets Manager RDS Secret](/images/lab1_setup_liquibase/aws_secrets_manager_done.png?width=600px&classes=border,shadow)
        * Note the __username__ at the end of the string.
        ![LIQUIBASE_COMMAND_USERNAME](/images/lab1_setup_liquibase/github_environment_var_3.png?width=600px&classes=border,shadow)
    * __LIQUIBASE_COMMAND_PASSWORD__ : Click the "Add environment variable" button.
        ![Add environment variable](/images/lab1_setup_liquibase/github_environment_var_4.png?width=600px&classes=border,shadow)
        * Set this in the form of  `aws-secrets,rds!cluster-AAA-BBB-CCC-DDD-ZZZ,password`. 
        * Replace string "rds!cluster-AAA-BBB-CCC-DDD-ZZZ" with your secret from AWS Secret Manager storing username and password for your RDS database.
        * Note the __password__ at the end of the string.
        ![LIQUIBASE_COMMAND_PASSWORD](/images/lab1_setup_liquibase/github_environment_var_5.png?width=600px&classes=border,shadow)

1. Your environment variables should look like this.
    ![Github environment variables](/images/lab1_setup_liquibase/github_environment_var_6.png?width=600px&classes=border,shadow)



## Configure Repository Variables

Follow the steps below to setup GitHub repository variables. These variable values stay constant and will not change from environment to environment.

1. On the left pane, click "Secrets and variables". Then click "Actions".

    ![GitHub secrets](/images/lab1_setup_liquibase/github_secrets_1.png?width=400px&classes=border,shadow)

1. Click on the "Variables" tab.

    ![GitHub repository variables](/images/lab1_setup_liquibase/github_variables_1.png?width=600px&classes=border,shadow)

1. Click "New repository variable" button.

    ![Create repository variables](/images/lab1_setup_liquibase/github_variables_2.png?width=600px&classes=border,shadow)

1. Create the following variables, one at a time:
    * __LIQUIBASE_COMMAND_CHANGELOG_FILE__ : Set this to `changelog.yaml`
        ![LIQUIBASE_COMMAND_CHANGELOG_FILE](/images/lab1_setup_liquibase/github_variables_3.png?width=600px&classes=border,shadow)
    * __MY_CONTAINER_NAME__ : Set this to `Liquibase-Pro`.
        ![MY_CONTAINER_NAME](/images/lab1_setup_liquibase/github_variables_7.png?width=600px&classes=border,shadow)
    * __MY_ECS_CLUSTER__ : Set this to the cluster name given in [Setup AWS ECS page](../020_self_guided_setup/025_setupawsecs.html), `workshop-cluster`.
        ![AWS ECS Cluster name](/images/lab1_setup_liquibase/aws_ecs_cluster_name.png?width=600px&classes=border,shadow)
        ![MY_ECS_CLUSTER](/images/lab1_setup_liquibase/github_variables_8.png?width=600px&classes=border,shadow)
    * __MY_ECS_TASK_DEFINITION__ : This is the name of the task definition file provided in the repository, `task-def-sidecar.json`. 
        ![MY_ECS_TASK_DEFINITION](/images/lab1_setup_liquibase/github_variables_9.png?width=600px&classes=border,shadow)
    * __IMAGE_NAME__ : The name of the Liquibase Pro image from AWS Market Place. This may look like this `709825985650.dkr.ecr.us-east-1.amazonaws.com/liquibase/liquibase/liquibasepro:4.30.0`:
        ![IMAGE_NAME](/images/lab1_setup_liquibase/github_variables_10.png?width=600px&classes=border,shadow)

1. Your GitHub Actions repository variables should look like this:
    ![GitHub repository variables completed](/images/lab1_setup_liquibase/github_variables_11.png?width=600px&classes=border,shadow)

Your GitHub repository is now configured with secrets, environment variables and repository variables. 

