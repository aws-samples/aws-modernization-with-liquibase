---
title: "Lab 5: Drift Detection"
chapter: true
weight: 7
---

# Lab 5 - Drift Detection

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

Drift detection is useful in determining what is different when deploying to a single database or determining what is different between two target databases to ensure that they are in sync.

In this lab, you will be updating the Liquibase flow file to create a snapshot of the database and storing it in the S3 bucket. 

The snapshot will be created (and updated) after each new update. Then, prior to the next update, automation would perform a database comparison by comparing the database against its saved snapshot to check if the database is still in the same known state as before, or if any changes were introduced between updates. 

Here is a preview of what we will be setting up:

1. Update flow file to store a snapshot in a JSON format to S3 bucket.
1. Update flow file to use the stored snapshot to compare the database.

Here are the two new stages we will be adding to the flow file: "__Compare__" and "__Snapshot__".
![Flow file new stages](/images/lab5_drift_detection/flow_file_w_snapshot_and_diff.png?width=400px&classes=border,shadow) 

We will first add the "__Snapshot__" stage. 