# Git stash

## Question

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/beta present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:

Look for the stashed changes under /usr/src/kodekloudrepos/beta git repository, and restore the stash with stash@{1} identifier. Further, commit and push your changes to the origin.

## Answer

- SSH into the storage server
- Change the desired location
```bash
ssh natasha@ststor01

sudo su -

cd /usr/src/kodeklouderepos/beta
```

- View the `stash list`
```bash
git stash list
```

- Apply the specific `stash@{1}` 
```bash
git stash apply stash@{1}
```

## Brief Description About the Environment

**Git stash**

This is for save current working progress and make chance to go around other branches without committing our work. We can `stash` in different times with `git stash`. Also we can list out all the stashes `git stash list`. Also we can get back to LAST_STASH `git stash pop`.

Like wise if we need to go with specific stash, We can use `git stash apply stash@{1}` this command.
