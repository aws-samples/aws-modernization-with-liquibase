---
title: "Update Environment Variables" # MODIFY THIS TITLE
chapter: true
weight: 62 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Update Environment Variables for UAT and PROD

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

In this section we will update GitHub environment variables. As such, we will create two new environments in GitHub:

1. __uat__ environment
1. __prod__ environment


## Create "uat" Environment and Variables

1. In your browser window, go to the LiquibaseFargatePOC Github repository. Click on the "Settings" tab.
    ![GitHub repository settings](/images/lab4_deploy_to_uat_prod/github_settings_1.png?width=800px&classes=border,shadow)

2. Click "Environment" in the left pane. Then click "New environments" button.

    ![GitHub environments](/images/lab4_deploy_to_uat_prod/github_environments_1.png?width=800px&classes=border,shadow)

1. Type __uat__ in the Name field and click "Configure environment".

    ![Dev](/images/lab4_deploy_to_uat_prod/github_environments_uat_1.png?width=600px&classes=border,shadow)

1. Scroll down to the __Environment variables__ section and click "Add environment variable" button.

    ![Add environment variable](/images/lab4_deploy_to_uat_prod/github_environments_uat_2.png?width=600px&classes=border,shadow)

1. Create the following variables, one at a time. 
    * Note that Liquibase environment variables referring to secrets in AWS Secrets Manager will use three-part name as follows: aws-secrets,_secretName_,_key_

    * __LIQUIBASE_COMMAND_URL__ : Set this to `aws-secrets,liquibase-workshop-secret,uat_url`.
        ![LIQUIBASE_COMMAND_URL](/images/lab4_deploy_to_uat_prod/github_environments_uat_3.png?width=600px&classes=border,shadow)
    * __LIQUIBASE_COMMAND_USERNAME__ : Click the "Add environment variable" button.
        ![Add environment variable](/images/lab4_deploy_to_uat_prod/github_environment_var_2.png?width=600px&classes=border,shadow)
        * Set this in the form of `aws-secrets,rds!cluster-AAAAA-BBB-CCCC-DDDD-ZZZZ,username`.
        * The string "rds!cluster-AAAAA-BBB-CCCC-DDDD-ZZZZ" should be replaced by the secret from your AWS Secret Manager storing username and password for your __UAT__ database.
        ![AWS Secrets Manager RDS Secret](/images/lab4_deploy_to_uat_prod/aws_secrets_manager_done_uat.png?width=600px&classes=border,shadow)
        * Note the __username__ at the end of the string.
        ![LIQUIBASE_COMMAND_USERNAME](/images/lab4_deploy_to_uat_prod/github_environment_var_3.png?width=600px&classes=border,shadow)
    * __LIQUIBASE_COMMAND_PASSWORD__ : Click the "Add environment variable" button.
        ![Add environment variable](/images/lab4_deploy_to_uat_prod/github_environment_var_4.png?width=600px&classes=border,shadow)
        * Set this in the form of  `aws-secrets,rds!cluster-AAA-BBB-CCC-DDD-ZZZ,password`. 
        * Replace string "rds!cluster-AAA-BBB-CCC-DDD-ZZZ" with your secret from AWS Secret Manager storing username and password for your RDS database.
        * Note the __password__ at the end of the string.
        ![LIQUIBASE_COMMAND_PASSWORD](/images/lab4_deploy_to_uat_prod/github_environment_var_5.png?width=600px&classes=border,shadow)

1. Your "uat" environment variables should look like this.
    ![Github environment variables](/images/lab4_deploy_to_uat_prod/github_environment_var_6.png?width=600px&classes=border,shadow)

## Create "prod" Environment and Variables

2. Click "Environment" in the left pane. Then click "New environments" button.

    ![GitHub environments](/images/lab4_deploy_to_uat_prod/github_environments_2.png?width=00px&classes=border,shadow)

1. Type __prod__ in the Name field and click "Configure environment".

    ![Dev](/images/lab4_deploy_to_uat_prod/github_environments_prod_1.png?width=600px&classes=border,shadow)

1. Scroll down to the __Environment variables__ section and click "Add environment variable" button.

    ![Add environment variable](/images/lab4_deploy_to_uat_prod/github_environments_prod_2.png?width=600px&classes=border,shadow)

1. Create the following variables, one at a time. 
    * Note that Liquibase environment variables referring to secrets in AWS Secrets Manager will use three-part name as follows: aws-secrets,_secretName_,_key_

    * __LIQUIBASE_COMMAND_URL__ : Set this to `aws-secrets,liquibase-workshop-secret,prod_url`.
        ![LIQUIBASE_COMMAND_URL](/images/lab4_deploy_to_uat_prod/github_environments_prod_3.png?width=600px&classes=border,shadow)
    * __LIQUIBASE_COMMAND_USERNAME__ : Click the "Add environment variable" button.
        ![Add environment variable](/images/lab4_deploy_to_uat_prod/github_environment_var_2.png?width=600px&classes=border,shadow)
        * Set this in the form of `aws-secrets,rds!cluster-AAAAA-BBB-CCCC-DDDD-ZZZZ,username`.
        * The string "rds!cluster-AAAAA-BBB-CCCC-DDDD-ZZZZ" should be replaced by the secret from your AWS Secret Manager storing username and password for your __PROD__ database.
        ![AWS Secrets Manager RDS Secret](/images/lab4_deploy_to_uat_prod/aws_secrets_manager_done_prod.png?width=600px&classes=border,shadow)
        * Note the __username__ at the end of the string.
        ![LIQUIBASE_COMMAND_USERNAME](/images/lab4_deploy_to_uat_prod/github_environment_var_3.png?width=600px&classes=border,shadow)
    * __LIQUIBASE_COMMAND_PASSWORD__ : Click the "Add environment variable" button.
        ![Add environment variable](/images/lab4_deploy_to_uat_prod/github_environment_var_4.png?width=600px&classes=border,shadow)
        * Set this in the form of  `aws-secrets,rds!cluster-AAA-BBB-CCC-DDD-ZZZ,password`. 
        * Replace string "rds!cluster-AAA-BBB-CCC-DDD-ZZZ" with your secret from AWS Secret Manager storing username and password for your RDS database.
        * Note the __password__ at the end of the string.
        ![LIQUIBASE_COMMAND_PASSWORD](/images/lab4_deploy_to_uat_prod/github_environment_var_5.png?width=600px&classes=border,shadow)

1. Your "prod" environment variables should look like this.
    ![Github environment variables](/images/lab4_deploy_to_uat_prod/github_environment_var_6.png?width=600px&classes=border,shadow)

