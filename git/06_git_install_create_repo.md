# Install Git and create repository

## Question

The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project. Create a Git repository on Storage server in Stratos DC as per details given below:

Install git package using yum on Storage server.

After that, create/init a git repository named /opt/cluster.git (use the exact name as asked and make sure not to create a bare repository).

## Answer


```bash
ssh natasha@ststr01

sudo yum update

sudo yum install git

sudo git init /opt/cluster.git

ls /opt/
```

## Brief Description About the Environment

**What is git?**

Git is a distributed version control system used to track changes in source code during software development. It allows multiple developers to collaborate, manage code history, and revert to previous versions efficiently.
