---
title: "Lab 3: Deploy To Dev"
chapter: true
weight: 4
---

# Lab 3 - Deploy to Dev 

In this lab, you will deploy database changes to your Dev database you provisioned earlier in [Setup AWS Aurora PostgreSQL](../020_self_guided_setup/023_setupawsaurorapostgres.html). 

Here is a preview of what we will be setting up:

1. Create a workflow in GitHub Actions
1. Access your job in AWS ECS
1. Access your logs in AWS CloudWatch

## Review Architecture Diagram

Let's revisit the architecture diagram to see how changes will arrive into the Dev database.

* Developers author database changes on their local IDE.
* Changes are committed into the GitHub repository, preferrably using a pull request which is approved by a DBA or data architect
    * In this workshop, we will commit changes directly into the _dev_ branch. 
    * However, in your workflow, you will likely have other branches that map to your specific environments.  
* A GitHub Action workflow is triggered which will perform several actions:
    * Package SQL scripts into a docker image and pushes the image to AWS ECR repository.
    * Execute a task definition in AWS ECS which starts two containers: SQL-Repository and Liquibase-Pro. A bind mount is used to share volume which enables Liquibase-Pro container to access SQL scripts from the SQL-Repository container.
    * Liquibase-Pro container uses database connection information from AWS Secrets Manager. This includes database URL, username and password.
    * Liquibase-Pro container executes Policy Checks using the checks-settings file we stored in the AWS S3 bucket in an earlier lab.
    * Once Policy Checks pass, Liquibase-Pro container will deploy changes to the Dev database.

![Architecture Diagram](/images/lab3_deploy_to_dev/architecture_diagram_dev.png?width=800px&classes=border,shadow) 

Previously, we configured a task definition with two containers. 

We also have all the pieces of AWS services in place: Dev database, secrets in AWS Secrets Manager and Policy Checks settings file in AWS S3 bucket.

And finally, we also configured GitHub secrets and variables, and a GitHub workflow file. 

We have all the pieces together now to trigger the GitHub Action to run the workflow file. 

