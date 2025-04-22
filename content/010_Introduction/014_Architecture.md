---
title: "Architecture Diagram" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 14 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# Architecture Diagram

Let's set some expectations about what we are setting up in this workshop.

From a high-level, a developer authors database changes with tools such as SQL Developer, SQL Server Mangement Studio or an Integrated Development Environment (IDE). These changes are committed into a source code repository, e.g., GitHub. Then, the database changes are deployed either automatically or on-demand into target database environments using Liquibase.

In this workshop we will setup three database environments __Dev__, __UAT__, and __PROD__. 

![Liquibase_Awards](/images/introduction/architecture_diagram_1.png?width=800px&classes=border,shadow) 

Let's take a closer look. GitHub Actions orchestrate deployment and invoke Liquibase. All updates to target database environments are conducted by Liquibase. Liquibase interacts with other parts of the AWS ecosystem, such as Amazon S3 and AWS Secrets Manager, in order to fetch Liquibase files and database credentials respectively. 

![Liquibase_Awards](/images/introduction/architecture_diagram_2.png?width=800px&classes=border,shadow) 

We will configure an AWS ECS cluster within AWS using a serverless infrastructure called AWS Fargate. A Fargate task will leverage two images to perform a database update:

* Container1 (SQL-Repository): Database changes from the GitHub repository are packaged as an image and pushed into AWS ECR. This image runs as a container inside an AWS Fargate Task. 

* Container2 (Liquibase-Pro): Your purchase of Liquibase Pro from AWS Marketplace is an image in AWS ECR which runs as a container inside the same AWS Fargate Task.

It is important to note that bind mounts are used to share data between the two containers. This is how the database changes are visible to Liquibase and can be used to be update the target database.

![Liquibase_Awards](/images/introduction/architecture_diagram_3.png?width=800px&classes=border,shadow) 

In the next few sections, you will follow instructions on how to set everything up, step by step.