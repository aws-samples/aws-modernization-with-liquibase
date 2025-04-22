---
title: "Configure Changelog" # MODIFY THIS TITLE
chapter: true
weight: 33 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Configure Changelog 

In this section, we will take a look at various changelog files that are included in the sample repository.

## Top-level changelog.yaml
The `changelog.yaml` file in the top-level directory is the entry point for Liquibase. 

![Top-level changelog.yaml](/images/lab1_setup_liquibase/top_level_changelog.png?width=400px&classes=border,shadow)

```yaml
databaseChangeLog:

# database1 = dvdrental
- include:
    file: database1/database1changelog.yaml
    relativeToChangelogFile: true

# database2 = unused
- include:
    file: database2/database2changelog.yaml
    relativeToChangelogFile: true

# database2 = unused
- include:
    file: database3/database3changelog.yaml
    relativeToChangelogFile: true
```

Liquibase identifies changes in a top-down fashion inside the changelog file. In our case, there are three "include" section in the changelog.yaml file. The first "include" tells Liquibase to traverse down to a child changelog file in database1 directory ("database1/database1changelog.yaml"). Changes in database1changelog.yaml will be deployed first before Liquibase traverses down to the second "include" section. 

## Child Changelogs
There are subdirectories for three databases ("database1", "database2" and "database3"). Each subdirectory contains a child changelog file. Let's take a look at the database1changelog.yaml file:

```yaml
databaseChangeLog:

- include:
    file: sequence/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: tables/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: index/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: functions/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: views/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: procedures/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: packages/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: packagebody/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: triggers/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: data/changelog.yaml
    relativeToChangelogFile: true
- include:
    file: materializedviews/changelog.yaml
    relativeToChangelogFile: true
```

Notice that there are additional "include" sections here. Each "include" points to another changelog.yaml file in a specific subdirectory. This series of "include" sections effectively tells Liquibase to find and deploy changes in this order: changes from "sequence/changelog.yaml" first, then changes from "tables/changelog.yaml", and so on.

Child changelog files in other database directories contain the same YAML code for traversing object subdirectories. 

Who is Responsible for Managing Changelog Files?

Top-level changelog, as well as all child changelog files are expected to stay in developer repository - same repository where SQL scripts are commited by database developers.


