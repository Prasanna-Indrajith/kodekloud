# git Merge Branch

## Question

The Nautilus application development team has been working on a project repository /opt/games.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

Create a new branch devops in /usr/src/kodekloudrepos/games repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.

## Answer

- ssh into Storage server 
```bash
ssh natasha@stator01

sudo su -
```

- go to specified directory and create new branch and switch to it.
```bash
cd /usr/src/kodekloudrepos/games

# Create and switch into new branch
git checkout -b devops master

cp /tmp/index.html .
```

- add the file to git and commit it
```bash
git add index.html

git commit -m "index.html file added"
```

- change the branch to `master` and merge the `devops` in to it
```bash
git checkout master

git merge devops
```

- Finally push all into remote server
```bash
git push
```

## Additional Details

Additional commands with git flow.

We can check the logs by `git log` and use `git status` also list down all the branches `git branch`
