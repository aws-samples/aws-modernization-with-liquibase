---
title: "Setup AWS ECR" # MODIFY THIS TITLE
chapter: true
weight: 26 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Setup Elastic Container Registry

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

You will now log into AWS Elastic Container Registry and create a repository for working in this workshop.

These are a few steps to complete:

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "ecr".  Then click "Elastic Container Registry" under Services:

![Console home search](/images/self_guided_setup/aws_ecr_1.png?width=800px&classes=border,shadow)

1. Create a new repository. Click "Create repositry" button.

![Create new repository](/images/self_guided_setup/aws_ecr_2.png?width=800px&classes=border,shadow)

1. Type the namespace/repo-name as "liquibase/sqlrepository". Ensure that "Mutable" option is selected. Then click "Create".

![New repository name](/images/self_guided_setup/aws_ecr_3.png?width=600px&classes=border,shadow)

1. Your new container repository is now setup.

![New repository](/images/self_guided_setup/aws_ecr_4.png?width=800px&classes=border,shadow)

