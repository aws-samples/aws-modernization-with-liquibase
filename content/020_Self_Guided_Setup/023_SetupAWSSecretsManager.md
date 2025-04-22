---
title: "Setup AWS Secrets Manager" # MODIFY THIS TITLE
chapter: true
weight: 23 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Setup AWS Secrets Manager

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

You will now validate that login information for your newly created Aurora PostgreSQL database are automatically created in AWS Secrets Manager.

These are a few steps to complete:

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "Secrets Manager".  Then click "Secrets Manager" under Services:

    ![Console home search](/images/self_guided_setup/aws_console_home_search_2.png?width=600px&classes=border,shadow)

1. Click the secret name which starts with "rds...":

    ![Secret name](/images/self_guided_setup/aws_secrets_manager_1.png?width=600px&classes=border,shadow)

1. You will now see details about your "rds..." secret name.

    ![Secret name details](/images/self_guided_setup/aws_secrets_manager_secret_details.png?width=600px&classes=border,shadow)

1.  In the Secret value section, click the "Retrieve secret value" to reveal the username and password for your Aurora PostgreSQL database:
    ![Secret name details](/images/self_guided_setup/aws_secrets_manager_secret_value.png?width=600px&classes=border,shadow)
    ![Secret name details](/images/self_guided_setup/aws_secrets_manager_secret_retrieved.png?width=600px&classes=border,shadow)

{{% notice info %}}
This secret is managed by Amazon RDS service. You will not be allowed to edit or change password here. You can learn more about [service linked secrets in Amazon docs](https://docs.aws.amazon.com/secretsmanager/latest/userguide/service-linked-secrets.html).
{{% /notice %}}

## Create a New Secret

We will now create a new secret in AWS Secrets Manager.

1. Click "Secrets" and then "Store a new secret" button.

![New Secret](/images/self_guided_setup/aws_secrets_manager_new_secret.png?width=600px&classes=border,shadow)

1. For Secret Type, choose "Other type of secret". In Key/value pair, type "dev_url" and the Endpoint for your RDS Postgres database. Append __jdbc:postgres://__ in the beginning of the string as follows: `jdbc:postgres://database-1-instance-1.cleuxxyyyzz11.us-west-2.rds.amazonaws.com`. Then click "+ Add Row" button.

    ![Secret Type](/images/self_guided_setup/aws_secrets_manager_url_1.png?width=600px&classes=border,shadow)

1. Add two more secrets: "uat_url" and "prod_url". We will leave their values empty for now.

    ![Secret Type](/images/self_guided_setup/aws_secrets_manager_url_2.png?width=600px&classes=border,shadow)

1. Click "Next".

    ![Secret Type](/images/self_guided_setup/aws_secrets_manager_url_3.png?width=600px&classes=border,shadow)


1. Give the secret name "liquibase-workshop-secret". Then click "Next". 

    ![Secret Type](/images/self_guided_setup/aws_secrets_manager_name.png?width=600px&classes=border,shadow)

1. Review the information and click "Store" at the bottom.

    ![Store secret](/images/self_guided_setup/aws_secrets_manager_store.png?width=600px&classes=border,shadow)

1. Your newly created secret is now ready for use later in this workshop.

    ![Store secret](/images/self_guided_setup/aws_secrets_manager_done.png?width=1000px&classes=border,shadow)
