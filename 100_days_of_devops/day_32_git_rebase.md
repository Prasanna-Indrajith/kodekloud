# Git rebase

## Question

The Nautilus application development team has been working on a project repository /opt/beta.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the feature branch with the master branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.

## Answer

- SSH into the storage server
- Change the desired location
```bash
ssh natasha@ststor01

sudo su -

cd /usr/src/kodeklouderepos/beta
```

- Checkout to `feature` branch and `rebase` it into `master` branch
```bash
git checkout feature

git rebase master
```

- Push changes into `feature` branch on remote server
```bash
git push origin feature --force-with-lease
```

## Brief Description About the Environment

**rebase**

**git rebase** reapplies commits from one branch onto another, creating a linear history by moving the base of your branch to a new commit.

Key points:
- **Replays commits**: it takes commits from your current branch that are not on the target and reapplies them onto the target.
- **Creates new commits**: replayed commits get new hashes (history is rewritten).
- Two main forms:
  - `git rebase <base-branch>` — move the current branch to start at the tip of <base-branch>.
  - `git rebase -i <base>` (interactive) — lets you pick, squash, reorder, edit, or drop commits during the rebase.

Common workflows:
- Update a feature branch with the latest main: checkout feature, `git rebase main`.
- Clean up commits before merging: `git rebase -i` to squash/fixup.

Conflict resolution:
- Resolve conflicts in files, then `git add <files>` and `git rebase --continue`.
- To stop and return to pre-rebase state: `git rebase --abort`.
- To skip a bad commit: `git rebase --skip`.

When to use:
- Use rebase to keep history linear and tidy for private/feature branches.
- Avoid rebasing commits that have already been pushed/shared with others (it rewrites history).

Tip: for a non-destructive alternative that preserves merge history, use `git merge` instead.

`--force-with-lease` is the crucial flag that provides a safer alternative to --force.
Unlike --force, which blindly overwrites the remote branch with your local branch, --force-with-lease adds a check.
It verifies that the remote branch's state (specifically, its last commit ID) is the same as what your local Git repository expects it to be based on your last fetch.
