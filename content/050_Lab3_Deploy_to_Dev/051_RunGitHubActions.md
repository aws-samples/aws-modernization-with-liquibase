---
title: "Run GitHub Actions" # MODIFY THIS TITLE
chapter: true
weight: 51 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Run GitHub Actions

In this lab, we will run a workflow in GitHub Actions and evaluate logs in AWS ECS and AWS CloudWatch.

1. Browse over to your "liquibaseFargatePOC" GitHub repository in your browser. Click "Actions" tab. 
![GitHub](/images/lab3_deploy_to_dev/github_1.png?width=800px&classes=border,shadow)

1. Notice that one or more workflows may have already executed. 
![GitHub Actions](/images/lab3_deploy_to_dev/github_2.png?width=800px&classes=border,shadow) 
    This is because the workflow file (aws.yml) is configured to trigger automatically when there is a commit to the __dev__ branch due to the following declaration in the workflow file:
    * __workflow_dispatch__ - This enabled user to manually dispatch the workflow based on code in the "dev" branch
    * __push__ - This enables GitHub Actions to automatically dispatch the workflow when code is pushed to the "dev" branch. 
![aws.yaml](/images/lab3_deploy_to_dev/aws_yaml_on.png?width==300px&classes=border,shadow) 

1. In the left pane, click "Deploy to Amazon ECS".
![Deploy to Amazon ECS](/images/lab3_deploy_to_dev/github_actions_deploy_to_amazon_ecs_1.png?width=800px&classes=border,shadow) 

1. Click the "Run workflow" pulldown and click the "Run workflow" button.
![Run workflow](/images/lab3_deploy_to_dev/github_actions_run_workflow_1.png?width=800px&classes=border,shadow) 

1. After a few seconds you will see a running workflow. Click the workflow title "Deploy to Amazon ECS" to access jobs in this workflow. 
![Running workflow](/images/lab3_deploy_to_dev/github_actions_running_1.png?width=800px&classes=border,shadow) 

1. The only job configured in this workflow is the "Deploy to dev" job. 
![Workflow in progress](/images/lab3_deploy_to_dev/github_actions_running_2.png?width=800px&classes=border,shadow) 

1. It takes a a minute to complete the "Deploy" job.  
![Workflow completed](/images/lab3_deploy_to_dev/github_actions_running_3.png?width=800px&classes=border,shadow) 

1. Upon completion, it starts running a task definition in AWS ECS. We will now switch to AWS ECS.
