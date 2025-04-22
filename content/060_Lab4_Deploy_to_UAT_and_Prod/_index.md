---
title: "Lab 4: Deploy to UAT & PROD"
chapter: true
weight: 6
---

# Lab 4 - Deploy to UAT and PROD 

{{% notice warning %}}
You are responsible for the cost of the AWS services used while running this workshop in your AWS account. We highly recommend you to go to the request AWS credit page so you can run this workshop without any charge to you.
{{% /notice %}}

In this lab, you will perform additional setup in AWS and GitHub Actions. 

Here is a preview of what we will be setting up:

1. Configure UAT and PROD databases in AWS RDS.
1. Update AWS Secrets Manager with UAT and PROD database credentials.
1. Update GitHub Actions to deploy to UAT and PROD environments.
1. Deploy to UAT and PROD environments.
1. Rollback changes in UAT and PROD environments.

## Review Workflow Diagram
Recall the workflow we discussed earlier that changes to UAT and PROD will come from merging changes to "uat" and "main" branches.
![Workflow Diagram](/images/lab4_deploy_to_uat_prod/workflow_diagram_1.png?width=800px&classes=border,shadow) 



