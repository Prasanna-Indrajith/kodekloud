# Delete git branch

## Question

The Nautilus developers are engaged in active development on one of the project repositories located at /usr/src/kodekloudrepos/demo. During testing, several test branches were created, and now they require cleanup. Here are the requirements provided to the DevOps team:

On the Storage server in Stratos DC, delete a branch named xfusioncorp_demo from the /usr/src/kodekloudrepos/demo Git repository.

## Answer

SSH into Storage Server 01 - natasha - ststor01
```bash
ssh natasha@ststor01
```

Get sudo privilages and cd to directory
```bash
sudo su -

cd /usr/src/kodekloudrepos/demo
```

Delete the branch xfusioncorp_demo
```bash
git branch -d xfusioncorp_demo
```

## Additional Details

Common usages:

- List all branches:
  ```
  git branch
  ```

- Create a new branch:
  ```
  git branch <branch-name>
  ```

- Delete a branch:
  ```
  git branch -d <branch-name>
  ```

- Rename a branch:
  ```
  git branch -m <old-name> <new-name>
  ```

- List all remote branches:
  ```
  git branch -r
  ```

Branches are pointers to commits, enabling parallel development and experimentation without affecting the main codebase.

## Brief Description About the Environment

**git branch**

The git branch command is used to manage branches in a Git repository. Branches allow you to work on different versions of your project simultaneously.
