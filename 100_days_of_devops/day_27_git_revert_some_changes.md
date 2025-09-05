# Git Revert Some Changes

## Question

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/media present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

In /usr/src/kodekloudrepos/media git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).

Use revert media message (please use all small letters for commit message) for the new revert commit.

## Answer

- Log into Storage Server and get the `sudo` access.
- Change the directory into /usr/src/kodekloudrepos/news
```bash
ssh natasha@ststor01

sudo su -

cd /usr/src/kodekloudrepos/media
```

- View the current commits and revert the last `HEAD` commit
```bash
git log

git revert HEAD
```

- Add new commit message without affecting previous commit message.
```bash
git commit --amend -m "revert media"
```

- Check newly added changes
```bash
git log
```

## Brief Description About the Environment

The `git commit --amend` command is a convenient way to modify the most recent commit. It lets you combine staged changes with the previous commit instead of creating an entirely new commit.

"This rewrites previous commit with entirely new commit. By providing `--no-edit` or `-m 'message'` we can simply change previous commit.

Git has several mechanisms for storing history and saving changes. These mechanisms include: Commit --amend, `git rebase` and `git reflog`

**Don’t amend public commits**

Amended commits are actually entirely new commits and the previous commit will no longer be on your current branch. This has the same consequences as resetting a public snapshot. Avoid amending a commit that other developers have based their work on. This is a confusing situation for developers to be in and it’s complicated to recover from.
