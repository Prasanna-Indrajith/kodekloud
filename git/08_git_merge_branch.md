# Git merge branch

## Question

The Nautilus application development team has been working on a project repository /opt/games.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

Create a new branch nautilus in /usr/src/kodekloudrepos/games repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.

## Answer

Here is the process !
```bash
ssh natasha@ststor01

cd /usr/src/kodekloudrepos/games

sudo su -

git checkout master

git checkout -b nautilus master

cp /tmp/index.html .

git add index.html

git commit - "index.html file added"

git checkout master

git merger nautilus

git push origin nautilus

git push origin master

exit
```
