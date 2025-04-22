---
title: "Setup AWS Account" # MODIFY THIS TITLE
chapter: true
weight: 21 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## AWS Account Setup

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

You will configure two items here:

* Create a "workshop" user in AWS.
* Create a custom policy for "workshop" user
* Configure "ecsTaskExecutionRole" Role
* Configure this user with programmatic access for use with Liquibase.

### Create a "workshop" user in AWS

1. If you don’t already have an AWS account with root access: [create one now by clicking here](https://aws.amazon.com/getting-started/).

1. Once you have an AWS account, login as root user to [create a new IAM user to use for the workshop](https://console.aws.amazon.com/iam/home?#/users$new). Next step will start from this link.

1. Enter the user details.
    ![Create user](/images/self_guided_setup/aws_create_user_1.png?width=800px&classes=border,shadow)
    ![Create user (click Next)](/images/self_guided_setup/aws_create_user_2.png?width=800px&classes=border,shadow)

1. Attach the "AdministratorAccess" IAM Policy.

    ![Attach policy](/images/self_guided_setup/aws_create_user_attach_policy_1.png?width=800px&classes=border,shadow)

1. Also attach the "CloudWatchLogsFullAccess" IAM Policy. Then click "Next":

    ![Attach policy](/images/self_guided_setup/aws_create_user_attach_policy_1.1.png?width=800px&classes=border,shadow)
    ![Attach policy (click Next)](/images/self_guided_setup/aws_create_user_attach_policy_2.png?width=800px&classes=border,shadow)

1. Click to create the new user.

    ![Finish user creation](/images/self_guided_setup/aws_create_user_finish_user_creation_1.png?width=800px&classes=border,shadow)

1. Copy the login URL:.

    ![login url](/images/self_guided_setup/aws_create_user_successful.png?width=800px&classes=border,shadow)

1. Open a new browser window and browse to the login URL. Enter the new username and password and click "Sign In".

    ![User login](/images/self_guided_setup/aws_login_user_1.png?width=300px&classes=border,shadow)

1. You will be prompted to change your password upon first login. Enter the new password and click "Confirm password change".

    ![Change password](/images/self_guided_setup/aws_login_new_password.png?width=400px&classes=border,shadow)

### Create a Custom Policy for "workshop" User

Let's go through adding custom policy for the "workshop" user. This will provide additional permissions:

1. From AWS Console Homepage, Click the Search bar and type "IAM" to access Identity Access Management services. Then click "IAM":

    ![Console home](/images/self_guided_setup/aws_console_home_1.png?width=600px&classes=border,shadow)
    ![IAM](/images/self_guided_setup/aws_iam_1.png?width=600px&classes=border,shadow)

1. From the left pane click "Users", then click "workshop" user.

    ![Users](/images/self_guided_setup/aws_iam_2.png?width=200px&classes=border,shadow)
    ![Workshop user](/images/self_guided_setup/aws_iam_workshop_user.png?width=600px&classes=border,shadow)

1. Select "Add permission" pulldown and select "Create inline policy".

    ![Add permissions](/images/self_guided_setup/aws_create_user_add_permissions_1.png?width=800px&classes=border,shadow)

1. Click on the "JSON" button.

    ![Create policy](/images/self_guided_setup/aws_create_user_create_policy_2.png?width=800px&classes=border,shadow)

1. And paste the following JSON:

    ```JSON
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "Statement1",
                "Effect": "Allow",
                "Action": [
                    "s3:*"
                ],
                "Resource": [
                    "arn:aws:s3:::aws-mp-list",
                    "arn:aws:s3:::aws-mp-list/*",
                    "arn:aws:s3:::liquibase-workshop-bucket",
                    "arn:aws:s3:::liquibase-workshop-bucket/*"
                ]
            },
            {
                "Sid": "licensemanager",
                "Effect": "Allow",
                "Action": [
                    "license-manager:*"
                ],
                "Resource": [
                    "*"
                ]
            },
            {
                "Sid": "Statement2",
                "Effect": "Allow",
                "Action": [
                    "secretsmanager:ListSecrets",
                    "secretsmanager:DescribeSecret",
                    "secretsmanager:GetRandomPassword",
                    "secretsmanager:GetResourcePolicy",
                    "secretsmanager:GetSecretValue",
                    "secretsmanager:ListSecretVersionIds",
                    "s3:ListBucket",
                    "s3:ListAllMyBuckets",
                    "s3:GetObject",
                    "ecr:GetAuthorizationToken",
                    "ecr:BatchCheckLayerAvailability",
                    "ecr:GetDownloadUrlForLayer",
                    "ecr:BatchGetImage",
                    "ecr:CompleteLayerUpload",
                    "ecr:UploadLayerPart",
                    "ecr:InitiateLayerUpload",
                    "ecr:PutImage",
                    "ecr:DescribeRepositories",
                    "ecr:CreateRepository",
                    "ecr:ListImages",
                    "ecr:DescribeImages",
                    "ecr:BatchDeleteImage",
                    "aws-marketplace:ListChangeSets",
                    "aws-marketplace:DescribeChangeSet",
                    "aws-marketplace:StartChangeSet",
                    "aws-marketplace:ListEntities",
                    "aws-marketplace:ListTasks",
                    "aws-marketplace:DescribeEntity",
                    "logs:CreateLogGroup"
                ],
                "Resource": [
                    "*",
                    "arn:aws:ecr:us-east-1:709825985650:repository/liquibase/liquibase/liquibasepro",
                    "arn:aws:aws-marketplace:us-east-2:804611071420:AWSMarketplace/ContainerProduct/prod-l2panlvbozc5e"
                ]
            }
        ]
    }
    ```

1. Here is what it should look like. Click "Next".

    ![Create policy](/images/self_guided_setup/aws_create_user_create_policy_3.png?width=800px&classes=border,shadow)

1. For Policy Name, type "AWSMPPolicies".

    ![AWSMPPolicies](/images/self_guided_setup/aws_create_user_create_policy_4.png?width=800px&classes=border,shadow)

    * Ensure that on the same page, these policies are listed correctly.

    ![AWSMPPolicies](/images/self_guided_setup/aws_create_user_create_policy_5.png?width=800px&classes=border,shadow)

    * If everything looks good, click "Create Policy" button.

    ![Create AWSMPPolicies policy](/images/self_guided_setup/aws_create_user_create_policy_6.png?width=800px&classes=border,shadow)

1. This is what your "workshop" permissions should look like.

    ![Finish user creation with AWSMPPolicies policy](/images/self_guided_setup/aws_create_user_finish_user_creation_2.png?width=800px&classes=border,shadow)

### Configure "ecsTaskExecutionRole" Role

A role is needed for AWS ECS task execution. This is the role needed (and passed to run-task operation) which runs the Liquibase Pro image in ECS.

Note the addition of AmazonECSTaskExecutionRole , SecretsManagerReadWrite and AmazonS3FullAccess. Policies for Secrets Manager and S3 are only required if Liquibase accesses these AWS services (e.g., search-path using s3:// or Secrets Manager for storing credentials). 

1. From the left pane click "Roles", then click "Create Role" button.

    ![Roles](/images/self_guided_setup/aws_create_role_1.png?width=200px&classes=border,shadow)
    ![Create role](/images/self_guided_setup/aws_create_role_2.png?width=800px&classes=border,shadow)

1. Select "AWS service". From use cases, select "Elastic Container Service". Choose "Elastic Continer Service Task". Then click "Next".

    ![Trusted entity type](/images/self_guided_setup/aws_create_role_3.png?width=800px&classes=border,shadow)

1. On Add Permission page, add these permission:
    1. AmazonECSTaskExecutionRolePolicy
        ![AmazonECSTaskExecutionRolePolicy](/images/self_guided_setup/aws_create_role_ecsexecution.png?width=800px&classes=border,shadow)

    1. AmazonS3FullAccess
        ![AmazonS3FullAccess](/images/self_guided_setup/aws_create_role_s3fullaccess.png?width=800px&classes=border,shadow)

    1. AWSMPPolicies
        ![AWSMPPolicies](/images/self_guided_setup/aws_create_role_awsmppolicies.png?width=800px&classes=border,shadow)

    1. CloudWatchLogsFullAccess
        ![CloudWatchLogsFullAccess](/images/self_guided_setup/aws_create_role_cloudwatchfullaccess.png?width=800px&classes=border,shadow)

    1. SecretsManagerReadWrite
        ![SecretsManagerReadWrite](/images/self_guided_setup/aws_create_role_secretmanager.png?width=800px&classes=border,shadow)

1. Click "Next".

    ![Create role](/images/self_guided_setup/aws_create_role_4.png?width=800px&classes=border,shadow)

1. Type role name as "ecsTaskExecutionRole".

    ![Role name](/images/self_guided_setup/aws_create_role_5.png?width=800px&classes=border,shadow)

1. Scroll down to Step 2 to make sure that all roles are successfully added. Then click "Create role".

    ![Role permissions](/images/self_guided_setup/aws_create_role_6.png?width=800px&classes=border,shadow)

1. Ensure that your newly created role shows up in the list of roles.

    ![New role](/images/self_guided_setup/aws_create_role_7.png?width=800px&classes=border,shadow)



### Configure "workshop" User With Programmatic Access

You will now setup programmatic access for this user. You will use this programmatic login from Liquibase to access AWS services such as AWS Secrets Manager and AWS S3.

1. From AWS Console Homepage, Click the Search bar and type "IAM" to access Identity Access Management services. Then click "IAM":

    ![Console home](/images/self_guided_setup/aws_console_home_1.png?width=600px&classes=border,shadow)
    ![IAM](/images/self_guided_setup/aws_iam_1.png?width=600px&classes=border,shadow)

1. From the left pane click "Users", then click "workshop" user.

    ![Users](/images/self_guided_setup/aws_iam_2.png?width=200px&classes=border,shadow)
    ![Workshop user](/images/self_guided_setup/aws_iam_workshop_user.png?width=600px&classes=border,shadow)

1. Click the "Security credentials" tab:

    ![Security credentials](/images/self_guided_setup/aws_iam_workshop_user_security.png?width=600px&classes=border,shadow)

1. Scroll down to the "Access keys (0)" section and click "Create access key":

    ![Create access key](/images/self_guided_setup/aws_iam_create_access_key.png?width=600px&classes=border,shadow)

1. Select "Other" option, then click "Next":

    ![Other](/images/self_guided_setup/aws_iam_other.png?width=600px&classes=border,shadow)

1. In "Set description tag" screen, type "Liquibase" for Description tag value. Then click "Create access key":

    ![Set description tag](/images/self_guided_setup/aws_iam_description.png?width=600px&classes=border,shadow)

1. On the following screen you will be shown two items: Access key and Secret access key. You will need to save these. Click the "Download .csv file" to save this locally on your machine.

    ![Download .csv file](/images/self_guided_setup/aws_iam_access_key.png?width=600px&classes=border,shadow)

1. You can open the downloaded .csv file in any text editor or as an Excel file. We will need these two keys later when we configure Liquibase.

    ![Access keys in a CSV file](/images/self_guided_setup/aws_iam_access_key_csv.png?width=600px&classes=border,shadow)

