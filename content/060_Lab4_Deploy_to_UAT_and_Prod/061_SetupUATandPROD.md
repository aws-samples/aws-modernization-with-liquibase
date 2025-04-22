---
title: "Setup UAT and PROD" # MODIFY THIS TITLE
chapter: true
weight: 61 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setup UAT and PROD Databases

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

In this section we will setup two more AWS RDS Aurora PostgreSQL databases. 

1. Setup RDS Aurora PostgreSQL for the UAT environment. 
1. Setup RDS Aurora PostgreSQL for the PROD environment.

## Setup RDS Aurora PostgreSQL for the UAT environment

1. Go to AWS Console Homepage. Click the Search bar and type "Aurora Postgres" and click "RDS".
![Console home search](/images/lab4_deploy_to_uat_prod/aws_console_home_search.png?width=600px&classes=border,shadow)

1. Go to "Databases" and click "Create Database" button.
![Create database](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_uat_1.png?width=800px&classes=border,shadow)

1. Select "Easy create" option. For "Engine type", select "Aurora (PostgreSQL Compatible)".
![Easy create](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_easy.png?width=600px&classes=border,shadow)

1. For "DB instance size" choose "Dev/Test" option.
![DB instance size](/images/lab4_deploy_to_uat_prod/aws_rds_configure_aurora_postgres_instancesize.png?width=600px&classes=border,shadow)

1. For "DB cluster identifier", type __uat__. 
![DB Cluster identifier and Master username](/images/lab4_deploy_to_uat_prod/aws_rds_configure_aurora_postgres_uat_id_username.png?width=600px&classes=border,shadow)
![Create database](/images/lab4_deploy_to_uat_prod/aws_rds_configure_aurora_postgres_create_database.png?width=600px&classes=border,shadow)

1. You may be prompted for suggested add-ons. No need to select any add-ons for this workshop. Click the "Close" button.
![Suggested add-ons](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_suggested_addons.png?width=600px&classes=border,shadow)

1. Congratulations! You have successfully created your UAT database.
![Database successfully created](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_uat_successful.png?width=800px&classes=border,shadow)

1. Finally, make sure to enable public access to this database. __Once the database instance is provisioned__, click the radio button for the instance > click the "Modify" button.
![Click Modify](/images/lab4_deploy_to_uat_prod/aws_rds_modify_uat_1.png?width=800px&classes=border,shadow)

1. Scroll down to Connectivity section > expand Additional.
![Advanced Configuration](/images/lab4_deploy_to_uat_prod/aws_rds_modify_connectivity.png?width=600px&classes=border,shadow)
![Publicly Accessible](/images/lab4_deploy_to_uat_prod/aws_rds_modify_publicly_accessible.png?width=600px&classes=border,shadow)

1. Scroll down and click Continue. 
![Publicly Accessible](/images/lab4_deploy_to_uat_prod/aws_rds_modify_continue.png?width=600px&classes=border,shadow)

1. On the next screen, select "Apply Immediate" and click Modify DB Instance.
![Publicly Accessible](/images/lab4_deploy_to_uat_prod/aws_rds_modify_apply_immediate_uat.png?width=600px&classes=border,shadow)

1. You can access the database now using the database instance. Click on the __uat-instance-1__ to access instance details. The Endpoint indicated is what we will use in Liquibase. ![Database successfully created](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_uat_endpoint.png?width=800px&classes=border,shadow)

Save this UAT Endpoint string. We will add this to AWS Secrets Manager in the next step.


## Setup RDS Aurora PostgreSQL for the PROD environment

1. Go to AWS Console Homepage. Click the Search bar and type "Aurora Postgres" and click "RDS".
![Console home search](/images/lab4_deploy_to_uat_prod/aws_console_home_search.png?width=600px&classes=border,shadow)

1. Go to "Databases" and click "Create Database" button.
![Create database](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_prod_1.png?width=800px&classes=border,shadow)

1. Select "Easy create" option. For "Engine type", select "Aurora (PostgreSQL Compatible)".
![Easy create](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_easy.png?width=600px&classes=border,shadow)

1. For "DB instance size" choose "Dev/Test" option.
![DB instance size](/images/lab4_deploy_to_uat_prod/aws_rds_configure_aurora_postgres_instancesize.png?width=600px&classes=border,shadow)

1. For "DB cluster identifier", type __prod__. 
![DB Cluster identifier and Master username](/images/lab4_deploy_to_uat_prod/aws_rds_configure_aurora_postgres_prod_id_username.png?width=600px&classes=border,shadow)
![Create database](/images/lab4_deploy_to_uat_prod/aws_rds_configure_aurora_postgres_create_database.png?width=600px&classes=border,shadow)

1. You may be prompted for suggested add-ons. No need to select any add-ons for this workshop. Click the "Close" button.
![Suggested add-ons](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_suggested_addons.png?width=600px&classes=border,shadow)

1. Congratulations! You have successfully created your UAT database.
![Database successfully created](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_prod_successful.png?width=800px&classes=border,shadow)

1. Finally, make sure to enable public access to this database. __Once the database instance is provisioned__, click the radio button for the instance > click the "Modify" button.
![Click Modify](/images/lab4_deploy_to_uat_prod/aws_rds_modify_prod_1.png?width=800px&classes=border,shadow)

1. Scroll down to Connectivity section > expand Additional.
![Advanced Configuration](/images/lab4_deploy_to_uat_prod/aws_rds_modify_connectivity.png?width=600px&classes=border,shadow)
![Publicly Accessible](/images/lab4_deploy_to_uat_prod/aws_rds_modify_publicly_accessible.png?width=600px&classes=border,shadow)

1. Scroll down and click Continue. 
![Publicly Accessible](/images/lab4_deploy_to_uat_prod/aws_rds_modify_continue.png?width=600px&classes=border,shadow)

1. On the next screen, select "Apply Immediate" and click Modify DB Instance.
![Publicly Accessible](/images/lab4_deploy_to_uat_prod/aws_rds_modify_apply_immediate_prod.png?width=600px&classes=border,shadow)

1. You can access the database now using the database instance. Click on the __prod-instance-1__ to access instance details. The Endpoint indicated is what we will use in Liquibase. ![Database successfully created](/images/lab4_deploy_to_uat_prod/aws_rds_create_database_prod_endpoint.png?width=800px&classes=border,shadow)

Save this PROD Endpoint string. We will add this to AWS Secrets Manager in the next step.

## Update AWS Secrets with URLs

We will now add URLs for the UAT and PROD databases to AWS Secrets Manager.

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "Secrets Manager".  Then click "Secrets Manager" under Services:
    ![Console home search](/images/lab4_deploy_to_uat_prod/aws_console_home_search_2.png?width=600px&classes=border,shadow)

1. You will see various secrets starting with "rds...". Click "liquibase-workshop-secret".
    ![Console home search](/images/lab4_de
    ploy_to_uat_prod/aws_secrets_1.png?width=800px&classes=border,shadow)
1. Click "Retrieve secret value" button. 
    ![Retrieve secret value](/images/lab4_deploy_to_uat_prod/aws_secrets_2.png?width=800px&classes=border,shadow)

1. Click "Edit" button.
    ![Edit secret](/images/lab4_deploy_to_uat_prod/aws_secrets_3.png?width=800px&classes=border,shadow)

1. Click the "+ Add row" button.
    ![Add row](/images/lab4_deploy_to_uat_prod/aws_secrets_4.png?width=800px&classes=border,shadow)

1. Type __uat_url__. The value should be the endpoint in this format: `jdbc:postgres://<uat_endpoint>/postgres`
1. Click the "+ Add row" button again.
1. Type __prod_url__. The value should be the endpoint in this format: `jdbc:postgres://<prod_endpoint>/postgres`
1. Click "Save".

    ![New secrets](/images/lab4_deploy_to_uat_prod/aws_secrets_5.png?width=800px&classes=border,shadow)

1. You should now have three key/value pairs stored in the secret. Click "Close".

    ![Close secrets](/images/lab4_deploy_to_uat_prod/aws_secrets_6.png?width=800px&classes=border,shadow)
