---
title: "Setup GitHub Repository" # MODIFY THIS TITLE
chapter: true
weight: 31 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setup GitHub Repository 

A sample repository is made available for this workshop. Click [liquibase-workshop-repo](https://github.com/adeelmalik78/Liquibase-workshop-repo) to take a look. 

In this section, you will create a new repository in GitHub and copy contents from the sample repository.

## Create A New Repository

Follow the steps below to clone this content into your own Github repository. 

1. Clone this repo using the __"git clone"__ command: `git clone https://github.com/adeelmalik78/Liquibase-workshop-repo`
    * Here is the basic tree structure of this repository. Inside the __sqlcode__ directory there are three subdirectories (__database1__, __database2__ and __database3__). For the purpose of this workshop we have SQL scripts in __database1__ directory only. 

    ```log
    .
    ├── Dockerfile
    ├── alive.sh
    ├── sqlcode
    │   ├── changelog.sql
    │   ├── changelog.yaml
    │   ├── flow.yaml
    │   ├── s3://liquibase-workshop-bucket/liquibase.checks-settings.conf
    │   │── liquibase.properties
    │   ├── database1
    │   │   ├── data
    │   │   ├── database1changelog.yaml
    │   │   ├── functions
    │   │   ├── index
    │   │   ├── materializedviews
    │   │   ├── packagebody
    │   │   ├── packages
    │   │   ├── procedures
    │   │   ├── sequence
    │   │   ├── tables
    │   │   ├── triggers
    │   │   └── views
    │   ├── database2
    │   │   ├── database2changelog.yaml
    │   └── database3
    │       └── database3changelog.yaml
    └── task-def-sidecar.json
    ```

1. Create a new GitHub repository, naming the repository __"liquibaseFargatePOC"__. 
![Create GitHub repository](/images/lab1_setup_liquibase/github_create_repo_1.png?width=600px&classes=border,shadow) 

1. Rename the cloned repository:
    ```shell
    mv Liquibase-workshop-repo liquibaseFargatePOC
    ```

1. __"cd"__ into the liquibaseFargatePOC directory and remove the __.git__ subdirectory.
    ```shell
    cd liquibaseFargatePOC
    rm -rf .git/
    ```

1. Create a new local repository and add, commit and push changes to the new remote repository.
    ```shell
    git init
    git add .
    git commit -m "first commit"
    git branch -M dev
    git remote add origin git@github.com:<owner_name>/liquibaseFargatePOC.git
    git push -u origin dev
    ```

Your new repository is now created.

Let's examine what else is in this repository.

## Liquibase Flow File

We have provided a sample Liquibase Flow file in the GitHub repository, __sqlcode/flow.yaml__. 

Flow file is a YAML file. In its simplistic form, it consists of one or more stages and an __endStage__ which always runs. The following flow file consists of 8 stages:

1. Version - Displays the version of Liquibase as well as all the drivers and extensions available to Liquibase.
1. Validation - Validates the changelog and identifies any possible errors with Liquibase syntax that may cause the __update__ command to fail.
1. Connect - Tests database access without requiring the changelog.
1. Checks - Runs __checks run__ command to execute Liquibase Pro Policy Checks against the changelog.
1. Status - Displays the number of undeployed changesets.
1. Update - Executes the __update-testing-rollback__ command. It performs three phases of updates: deploying changes, rolling them back and then re-deploying them again.
1. Rollback - Executes the __rollback-one-update__ command. This rolls back all the changes that were just deployed.
1. endStage - Executes the __history__ command.
```yaml
##########           LIQUIBASE FLOWFILE                ##########
##########  learn more http://docs.liquibase.com/flow  ##########

stages:

  Version:
    actions:
      - type: shell
        command: liquibase --version

  Validation:
    actions: 
      - type: liquibase
        command: validate

  Connect:
    actions:
      - type: liquibase
        command: connect

  Checks:
    actions:
      - type: liquibase
        command: checks show
        cmdArgs: {
          check-status: enabled,
          auto-update: off,
          checks-settings-file: "s3://liquibase-workshop-bucket/liquibase.checks-settings.conf"
        }

      - type: liquibase
        command: checks run
        cmdArgs: {
          auto-update: off,
          changeset-filter: pending,
          checks-settings-file: "s3://liquibase-workshop-bucket/liquibase.checks-settings.conf"
        }

  Status:
    actions:
      - type: liquibase
        command: status
        cmdArgs: {verbose: true}

  Update:
    actions:
      - type: liquibase
        command: update

## The endStage ALWAYS RUNS. 
## So put actions here which you desire to perform whether previous stages' actions succeed or fail.
## If you do not want any actions to ALWAYS RUN, simply delete the endStage from your flow file.

endStage:
  actions:
    - type: liquibase
      command: history
```

## Changelog File

The entry point changelog file is provided as __sqlcode/changelog.yaml__ file. 

This changelog points to child changelog in __database1/database1changelog.yaml__ which points to additional changelogs in specific subdirectories.

## SQL scripts

All the SQL scripts are provided in the __sqlcode/database1__ directory. Scripts are organized into objects subdirectories, such as __tables__, __index__, __data__, and so on.

```shell
├── sqlcode
│   ├── changelog.yaml
│   ├── database1
│   │   ├── data
│   │   ├── database1changelog.yaml
│   │   ├── functions
│   │   ├── index
│   │   ├── materializedviews
│   │   ├── packagebody
│   │   ├── packages
│   │   ├── procedures
│   │   ├── sequence
│   │   ├── tables
│   │   ├── triggers
│   │   └── views
```

We will now setup secrets and variables in the GitHub repository.

<!-- ## Configure Repository Secrets

Follow the steps below to setup GitHub repository secrets which will enable GitHub to authenticate IAM user.

1. In your browser window, go to your Github repository. 

    * Go to "Settings"

    ![GitHub repository settings](/images/lab1_setup_liquibase/github_settings_1.png?width=800px&classes=border,shadow)

    * On the left pane, click "Secrets and variables". Then click "Actions".

    ![GitHub secrets](/images/lab1_setup_liquibase/github_secrets_1.png?width=400px&classes=border,shadow)

1. In the "Repository secrets" section, click "New repository secret" button. Create these four secrets, one at a time.

    * __AWS_REGION__ (this is the AWS region where you have setup AWS infrastructure)
    * __AWS_ACCESS_KEY_ID__ (this was in a CSV file in [Configure “workshop” User With Programmatic Access](../020_self_guided_setup/021_setupawsaccount.html)) 
    * __AWS_SECRET_ACCESS_KEY__ (this was saved in a CSV file in [Configure “workshop” User With Programmatic Access](../020_self_guided_setup/021_setupawsaccount.html))
    * __AWS_ACCOUNT_NUMBER__ (this is your 12-digit AWS account number in the top right corner of your AWS pages in the browser)

    ![GitHub repository secrets](/images/lab1_setup_liquibase/github_secrets_2.png?width=800px&classes=border,shadow)
    ![GitHub repository secrets](/images/lab1_setup_liquibase/github_secrets_3.png?width=600px&classes=border,shadow)
    ![GitHub repository secrets](/images/lab1_setup_liquibase/github_secrets_4.png?width=600px&classes=border,shadow)

Your GitHub repository is now configured with sample SQL scripts in the __sqlcode__ directory as well as the Liquibase Flow file __sqlcode/flow.yaml__. 
 -->
