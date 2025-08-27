# Git Create Branches

## Question

Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/ecommerce. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:

On Storage server in Stratos DC create a new branch xfusioncorp_ecommerce from master branch in /usr/src/kodekloudrepos/ecommerce git repo.
Please do not try to make any changes in the code.

## Answer

- ssh into storage server
```bash
ssh natasha@ststor01

sudo su -
```

- Change the directory into `/usr/src/kodekloudrepos` and check the available Branches
```bash
cd /usr/src/kodekloudrepos

git checkout
```

- Change the branch to master and create new branch from master branch
```bash
git checkout master

git checkout -b xfusioncorp_ecommerce master

git checkout
```

## Additional Details

Additional commands with

Create branch from another branch and use that branch
```bash
sudo git checkout -b <new-branch-name> <which-branch-to-clone>
```
