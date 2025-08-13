# Git Creat branch

## Question

Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/beta. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:

On Storage server in Stratos DC create a new branch xfusioncorp_beta from master branch in /usr/src/kodekloudrepos/beta git repo.

Please do not try to make any changes in the code.

## Answer


```bash
ssh natasha@ststor01

cd /usr/src/kodekloudrepos/beta

sudo git checkout -b xfusioncorp_beta master

sudo git branch -a
```

## Additional Details

Additional commands with

Create branch from another branch and use that branch
```bash
sudo git checkout -b <new-branch-name> <which-branch-to-clone>
```
