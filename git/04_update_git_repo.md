# Update git repository with adding sample HTML file

## Question

The Nautilus development team has initiated a new project development, establishing various Git repositories to manage each project's source code. Recently, a repository named /opt/cluster.git was created. The team has provided a sample index.html file located on the jump host under the /tmp directory. This repository has been cloned to /usr/src/kodekloudrepos on the storage server in the Stratos DC.

Copy the sample index.html file from the jump host to the storage server placing it within the cloned repository at /usr/src/kodekloudrepos/cluster.

Add and commit the file to the repository.

Push the changes to the master branch.

## Answer

Copy /tmp/index.html file into Storage server's /tmp directory - natasha - ststor01
```bash
sudo scp /tmp/index.html natasha@ststor01:/tmp/
```

SSH into Storage Server
```bash
ssh natasha@ststor01
```

Get sudo privilages
```bash
sudo su -
```

Move the index file to /usr/src/kodekloudrepos/cluster
```bash
mv /tmp/index.html /usr/src/kodekloudrepos/cluster/
```

Add index.html file to git repository
```bash
git add index.html
```

Commit the changes
```bash
git commit -m "index.html file added"
```

Push the repository to master branch in remote repository
```bash
git push
```

## Additional Details

Additional commands with `git`

See current status at each step
```bash
git status
```

## Brief Description About the Environment

**git add, git commit, git push**

- **git add**: Stages changes (new, modified, or deleted files) to be included in the next commit. It prepares your working directory changes for committing.

- **git commit**: Records the staged changes in the repository’s history. Each commit creates a snapshot of your project and includes a message describing the changes.

- **git push**: Uploads your local commits to a remote repository (like GitHub), making your changes available to others and backing up your work online.

The **scp** (secure copy) command in Linux is used to securely transfer files or directories between local and remote systems over SSH. It encrypts the data during transfer, ensuring security. The basic syntax is:

```sh
scp source_file user@remote_host:/destination/path
```

You can also use scp to copy files from a remote server to your local machine or between two remote servers.

