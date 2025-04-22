---
title: "Setup AWS Aurora PostgreSQL" # MODIFY THIS TITLE
chapter: true
weight: 22 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---


## Setup AWS Aurora PostgreSQL database

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

If you haven't logged in, use the login URL from [Setup AWS Account page](/en/020_self_guided_setup/021_setupawsaccount.html). 

You will use Amazon RDS to create new a new Aurora PostgreSQL database. 

This will be our __Dev__ database environment. In a later lesson, we will also setup __UAT__ and __PROD__ database environments. 

These are the few steps to complete to set up your Dev database.

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar:
![Console home](/images/self_guided_setup/aws_console_home_1.png?width=600px&classes=border,shadow)

1. Type "Aurora Postgres" in the search bar and click "RDS".
![Console home search](/images/self_guided_setup/aws_console_home_search.png?width=600px&classes=border,shadow)

1. Go to "Databases" and click "Create Database".
![Create database](/images/self_guided_setup/aws_rds_create_database.png?width=800px&classes=border,shadow)

1. Select "Easy create" option. For "Engine type", select "Aurora (PostgreSQL Compatible)".
![Easy Create](/images/self_guided_setup/aws_rds_create_database_easy.png?width=600px&classes=border,shadow)

1. For "DB instance size", choose "Dev/Test" option.
![DB Instance size](/images/self_guided_setup/aws_rds_configure_aurora_postgres_instancesize.png?width=600px&classes=border,shadow)

1. For "DB cluster identifier", type __dev__. 
![DB Cluster identifier and Master username](/images/self_guided_setup/aws_rds_configure_aurora_postgres_id_username.png?width=600px&classes=border,shadow)
![Create database](/images/self_guided_setup/aws_rds_configure_aurora_postgres_create_database.png?width=600px&classes=border,shadow)

1. You may be prompted for suggested add-ons. No need to select any add-ons for this workshop. Click the "Close" button.
![Suggested add-ons](/images/self_guided_setup/aws_rds_create_database_suggested_addons.png?width=600px&classes=border,shadow)

1. Congratulations! You have successfully created your Amazon Aurora PostgreSQL database.
![Database successfully created](/images/self_guided_setup/aws_rds_create_database_successful.png?width=800px&classes=border,shadow)

1. Finally, make sure to enable public access to this database. __Once the database instance is provisioned__, click the radio button for the instance > click the "Modify" button. 
![Click Modify](/images/self_guided_setup/aws_rds_modify_1.png?width=800px&classes=border,shadow)

1. Scroll down to Connectivity section > expand Additional.
![Advanced Configuration](/images/self_guided_setup/aws_rds_modify_connectivity.png?width=600px&classes=border,shadow)
![Publicly Accessible](/images/self_guided_setup/aws_rds_modify_publicly_accessible.png?width=600px&classes=border,shadow)

1. Scroll down and click Continue. 
![Publicly Accessible](/images/self_guided_setup/aws_rds_modify_continue.png?width=600px&classes=border,shadow)

1. On the next screen, select "Apply Immediate" and click Modify DB Instance.
![Publicly Accessible](/images/self_guided_setup/aws_rds_modify_apply_immediate.png?width=600px&classes=border,shadow)

1. You can access the database now using the database instance. Click on the __dev-instance-1__ to access instance details. The Endpoint indicated is what we will use in Liquibase. 
![Database successfully created](/images/self_guided_setup/aws_rds_create_database_endpoint.png?width=800px&classes=border,shadow)

Save this Endpoint string. We will add this to AWS Secrets Manager in the next step.


