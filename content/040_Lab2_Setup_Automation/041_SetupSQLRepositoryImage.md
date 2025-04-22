---
title: "Setup SQL Repository Image" # MODIFY THIS TITLE
chapter: true
weight: 41 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setup SQL-Repository Image

The SQL-Repository image will consist of new SQL script which developers have merged. In essence, we can simply package all files from the __sqlcode__ directory into a docker image using the a Dockerfile.

A __"Dockerfile"__ is already provided in the GitHub repository you cloned in the previous lab.
    ![Dockerfile](/images/lab2_setup_automation/Dockerfile_1.png?width=800px&classes=border,shadow)

Some comments about this Dockerfile:

1. We only include contents from the __sqlcode__ directory.
1. We copy a script __alive.sh__ into the docker image. Upon running this image, we will run this shell script to ensure that the docker containers keeps running.
1. We make the __/sqlcode__ directory inside the docker image available to be shared with the host or other docker containers running on the same host. This is what enables sharing SQL scripts from this image to Liquibase Pro image.

Here are the contents of this file:
```dockerfile
FROM ubuntu:18.04
RUN apt-get update
COPY sqlcode /sqlcode/
COPY alive.sh .
VOLUME [ "/sqlcode" ]
```

This is what the __alive.sh__ script looks like:

1. It enters a WHILE loop. Each iteration of the WHILE loop:
    1. Prints out a banner and the contents of the current working directory ("__pwd__").
    1. Sleeps for 15 seconds.

```shell
while [ 1 ]
do
    echo "************************* sqlrepository is alive *************************"
    echo "Current working directory is : `pwd`"
    ls -alh alive.sh /sqlcode
    # ls -alh ./sqlcode
    sleep 15
done
```

In order to build this docker image locally, you could run the following __docker__ commands:

```shell
docker build -t sqlrepository:latest .
```


However, we do not need to build this docker image locally. We will need to build this as part of GitHub Actions automation. 

In the next section we will add these commands in the GitHub Actions yaml file to build, tag and push this docker images to Amazon ECR repository __liquibase/sqlrepository__ which we previously configured in [Setup AWS ECR page](/020_self_guided_setup/026_setupawsecr.html).  

Since we did not make any changes to any files, there is no need to commit changes to the GitHub repository.