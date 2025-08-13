# Git revert some changes

## Question

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/demo present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

In /usr/src/kodekloudrepos/demo git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).

Use revert demo message (please use all small letters for commit message) for the new revert commit.

## Answer


```bash
ssh  natasha@ststor01

cd /usr/src/kodekloudrepos/demo

sudo su

git log

git revert HEAD
    -> "revert demo"

git log
```

## Brief Description About the Environment

**Why use `git revert`:**
- To undo a specific commit by creating a new commit that reverses its changes.
- It preserves project history (does not remove commits), making it safe for shared/public branches.
- Useful for undoing changes without rewriting history.

**When to use `git revert`:**
- When you need to undo a commit that has already been pushed to a shared repository.
- When you want to maintain a clear, auditable history of changes.
- When collaborating with others and want to avoid disrupting their work (unlike `git reset`).

**Summary:**  
Use `git revert` to safely undo commits on branches others may use, especially after pushing to remote.



