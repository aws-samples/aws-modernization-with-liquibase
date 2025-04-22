---
title: "Lab 2: Setup Automation"
chapter: true
weight: 4
---

# Lab 2 - Setup Automation 

In this lab, you will setup your GitHub Actions for use with Liquibase. 

Here is a preview of what we will be setting up:

1. Setup SQL Repository docker image
1. Configure Actions in your GitHub repository
1. Setup a task definition file for use with Amazon ECS

## Review Architecture Diagram

Before we start, recall from the [Architecture Diagram page](..010_introduction/014_architecture.html) that we will be configuring two containers to run simultaneously and use bind mounts to share data between the two containers. One container would be called __SQL-Repository__ and the other would be called __Liquibase-Pro__. 

![Architecture Diagram](/images/introduction/architecture_diagram_3.png?width=800px&classes=border,shadow) 

We already have one of the two containers configured in [Setup AWS ECS page](../020_self_guided_setup/025_setupawsecs.html) where we configured a task definition called __liquibase_flow__. This task definition was configured with a single image, the Liquibase Pro image, from AWS Market Place. When we deploy this task definition, we would be running __Liquibase-Pro__ container only. 

![AWS ECS task definition](/images/lab2_setup_automation/aws_ecs_task_definition_1.png?width=800px&classes=border,shadow) 


In this section, we will create the __SQL-Repository__ image and update our task definition to use this image.