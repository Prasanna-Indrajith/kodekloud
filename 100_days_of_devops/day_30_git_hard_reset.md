# Git hard Reset

## Question

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/ecommerce present on Storage server in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details:

In /usr/src/kodekloudrepos/ecommerce git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file.

Also make sure to push your changes.

## Answer

- SSH into the storage server
- Change the desired location
```bash
ssh natasha@ststor01

sudo su -

cd /usr/src/kodeklouderepos/ecommerce
```

- View the log file and find the `hash` related to 'add data.txt'
```bash
git log --oneline
```

- Hard reset the repository into that "hash" position
```bash
git reset --hard <hash>
```

- `push` into remote repository forcefully
```bash
git push origin master --forece
```

## Brief Description About the Environment

**Git hard reset**

**git reset --hard** moves the current branch tip, the index (staging area), and the working directory to match a specified commit, discarding any staged or unstaged changes. It rewrites history for the branch pointer and can permanently lose uncommitted work.

Examples:
- `git reset --hard` — reset to HEAD (discard local changes).
- `git reset --hard <commit>` — make the branch, index, and working tree match <commit>.

Warning: **this permanently deletes uncommitted changes** (unless recoverable via reflog for committed changes).
