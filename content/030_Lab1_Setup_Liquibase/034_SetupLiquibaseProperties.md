---
title: "Setup liquibase.properties" # MODIFY THIS TITLE
chapter: true
weight: 34 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Setup liquibase.properties File

__WE WILL LIKELY NOT NEED THIS SECTION__

In this section, we will create a `liquibase.properties` file and place in our S3 bucket ("liquibase-workshop-bucket").

These are a few steps to complete:

1. Create a new file locally and give it the name `liquibase.properties`. Place the following content into this file. Be sure to copy the database endpoint from [Setup AWS Aurora PostgreSQL database](en/020_self_guided_setup/023_setupawsaurorapostgres.html) page into `hostname`:

    ```
    changelogFile: changelog.yaml

    url: jdbc:postgresql://hostname:5432/postgres
    username: aws-secrets,aurora-postgres/username,username
    password: aws-secrets,aurora-postgres/password,password

    liquibase.search-path=.,S3://liquibase-workshop-bucket
    ```

    ![Console home search](/images/lab1_setup_liquibase/liquibase.properties_1.png?width=400px&classes=border,shadow)


1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "S3".  Then click "S3" under Services. 
    * Click on the S3 bucket you previously created ("liquibase-workshop-bucket").
        ![Console home search](/images/lab1_setup_liquibase/aws_console_home_search_3.png?width=600px&classes=border,shadow)
        ![Console home search](/images/lab1_setup_liquibase/aws_s3_liquibase_workshop_bucket.png?width=600px&classes=border,shadow)

1. Click the "Upload" button:
![Console home search](/images/lab1_setup_liquibase/aws_s3_upload.png?width=600px&classes=border,shadow)

1. Use the mouse to drag the `liquibase.properties` file to the "upload area". Or, use the "upload" button to upload the file.
![Console home search](/images/lab1_setup_liquibase/aws_s3_upload_liquibase.properties_1.png?width=600px&classes=border,shadow)
![Console home search](/images/lab1_setup_liquibase/aws_s3_upload_liquibase.properties_2.png?width=600px&classes=border,shadow)

1. Be sure to click the "Upload" button to confirm uploading the file to S3:
![Console home search](/images/lab1_setup_liquibase/aws_s3_upload_liquibase.properties_3.png?width=600px&classes=border,shadow)

1. Once the file is successfully uploaded, your "liquibase-workshop-bucket" will show the liquibase.properties file:
![Console home search](/images/lab1_setup_liquibase/aws_s3_upload_liquibase.properties_4.png?width=600px&classes=border,shadow)


