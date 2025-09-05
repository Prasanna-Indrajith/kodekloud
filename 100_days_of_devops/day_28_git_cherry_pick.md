# Git Cherry Pick

## Question

The Nautilus application development team has been working on a project repository /opt/news.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:



There are two branches in this repository, master and feature. One of the developers is working on the feature branch and their work is still in progress, however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is Update info.txt. Accomplish this task for them, also remember to push your changes eventually.

## Answer

- Login to Storage server and go to desired location
```bash
ssh natasha@ststor01

sudo su -

cd /usr/src/kodekloudrepos/news
```

- View current branch `feature` log and copy the hash, which is related to message "Update info.txt"

- Go to `master` branch
```bash
git log --oneline

git checkout master
```

- Merge the commit that we copied the hash earlier.
```bash
git cherry-pick <copied-hash>
```

- Push the updates to remote repository
```bash
git push
```

## Additional Details

Additional commands with `git`

- `git log --oneline`
- `git status`
- `git branch`
- `git checkout <branch-name>`
- `git cherry-pick <hash>`

## Brief Description About the Environment

`git cherry-pick` copies the changes introduced by one (or more) existing commit(s) onto your current branch as new commit(s). Use it to apply specific commits from another branch without merging the whole branch.

Key commands:
- `git cherry-pick <commit>` — apply a single commit.
- `git cherry-pick <A> <B> ...` — apply multiple commits.
- `git cherry-pick -x <commit>` — append “(cherry picked from commit ...)” to the new commit message.
- `git cherry-pick --continue`, `--abort`, `--quit` — control flow when resolving conflicts.

Notes: it creates new commits (different hashes), can cause conflicts that you must resolve, and is best for isolated changes rather than large, dependent histories.
