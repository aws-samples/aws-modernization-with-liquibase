---
title: "Setup AWS S3 Bucket" # MODIFY THIS TITLE
chapter: true
weight: 24 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Setup S3 Bucket

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

You will now log into AWS S3 and create a storage bucket for files you will be storing in this workshop.

These are a few steps to complete:


1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "S3".  Then click "S3" under Services:

![Console home search](/images/self_guided_setup/aws_console_home_search_3.png?width=600px&classes=border,shadow)

1. You will create a new bucket. Click "Create bucket" button:

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_1.png?width=600px&classes=border,shadow)

1. Select "General purpose" bucket type. Type in "liquibase-workshop-bucket" as your bucket name.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_2.png?width=600px&classes=border,shadow)

1. Select "ACL disabled" for object ownership. This selection will ensure that all objects in this bucket are owned by this account.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_3.png?width=600px&classes=border,shadow)

1. Select "Block all public access" for this bucket for the purpose of this workshop.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_4.png?width=600px&classes=border,shadow)

1. "Disable" bucket versioning.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_5.png?width=600px&classes=border,shadow)

1. No changes needed in Tags section.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_6.png?width=600px&classes=border,shadow)

1. Select "Server-side encryption" and "Enable" bucket key.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_7.png?width=600px&classes=border,shadow)

1. Skip any advanced settings and click "Create bucket" button.

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_8.png?width=600px&classes=border,shadow)

1. Congratulations! You have successfully created your AWS S3 bucket called "liquibase-workshop-bucket".

![Console home search](/images/self_guided_setup/aws_s3_create_bucket_9.png?width=900px&classes=border,shadow)

