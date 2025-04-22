---
title: "Setup Policy Checks" # MODIFY THIS TITLE
chapter: true
weight: 35 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Setup Liquibase Policy Checks

### Liquibase Policy Checks

Policy Checks can enforce many areas of responsibilities in database code, such as data in the database, database performance or database access. In addition, Policy Checks can also be used to enforce that sufficient Liquibase metadata is included, such as whether there is a rollback provided for each change.

### Configure Liquibase Policy Checks
Liquibase Policy Checks are store in a file called "checks settings file". This file is configured interactively from Command Line Interface. 

In the "Liquibase-workshop-repo", we have provided a pre-configured checks settings file called `liquibase.checks-settings.conf`. 

![liquibase.checks-settings.conf file](/images/lab1_setup_liquibase/liquibase_checks_settings_file.png?width=400px&classes=border,shadow)

### Who Manages "Checks Settings File"?
Checks settings files are typically the responsibility of automation engineers and DBAs depending on the types of checks to be performed. 

As such, checks settings files need to stay out of the developer repository. An ideal place for checks settings file is the S3 bucket.

Following steps will help you put the provided `liquibase.checks-settings.conf` file into our previously configured S3 bucket.

1. Go to [AWS Console Homepage](https://console.aws.amazon.com/). Click the Search bar and type "S3".  Then click "S3" under Services. 
    * Click on the S3 bucket you previously created ("liquibase-workshop-bucket").
        ![Console home search](/images/lab1_setup_liquibase/aws_console_home_search_3.png?width=800px&classes=border,shadow)
        ![Select S3 bucket](/images/lab1_setup_liquibase/aws_s3_liquibase_workshop_bucket.png?width=800px&classes=border,shadow)

2. Click the "Upload" button:
![Press the Upload button](/images/lab1_setup_liquibase/aws_s3_upload.png?width=600px&classes=border,shadow)

3. Use the mouse to drag the `liquibase.checks-settings.conf` file to the "upload area". Or, use the "upload" button to upload the file.
    ![Drag the file here](/images/lab1_setup_liquibase/aws_s3_upload_liquibase_checks_settings_conf_1.png?width=800px&classes=border,shadow)
    ![Drag the file here](/images/lab1_setup_liquibase/aws_s3_upload_liquibase_checks_settings_conf_2.png?width=800px&classes=border,shadow)

4. Be sure to click the "Upload" button to confirm uploading the file to S3:
    ![Upload confirmation](/images/lab1_setup_liquibase/aws_s3_upload_liquibase_checks_settings_conf_3.png?width=600px&classes=border,shadow)

5. Once the file is successfully uploaded,  your "liquibase-workshop-bucket" will show the `liquibase.checks-settings.conf` file:
    ![Upload complete](/images/lab1_setup_liquibase/aws_s3_upload_liquibase_checks_settings_conf_4.png?width=800px&classes=border,shadow)
    ![Upload confirmed](/images/lab1_setup_liquibase/aws_s3_upload_liquibase_checks_settings_conf_5.png?width=800px&classes=border,shadow)

