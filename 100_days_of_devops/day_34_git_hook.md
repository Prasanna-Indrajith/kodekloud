# Git Hook

## Question

The Nautilus application development team was working on a git repository /opt/cluster.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. The team want to setup a hook on this repository, please find below more details:

Merge the feature branch into the master branch`, but before pushing your changes complete below point.

Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release.

Finally remember to push your changes.

## Answer

- Get super user Secure Shell Access to Storage server
```bash
ssh natasha@ststor01

sudo su -
```

- We need to create this HOOK in remote git repository (In github like Environment we can do it by `workflow`. Now We need to navigate to `apps.git/hooks`
```bash
cd /opt/cluster.git/hooks

ls
  > "We already have post-update.sample file."
```

- Rename the file `post-update.sample` file into `post-update`
```bash
mv post-update.sample post-update
```

- Edit the file and add this bash script
```bash
vi post-update
```

```post-update
#!/bin/bash

BRANCH=$(git rev-parse --abbrev-ref HEAD)

if [ "$BRANCH" == "master" ]; then
    TODAY=$(date +%Y-%m-%d)
    git tag "release-${TODAY}"
fi
```

- Make that file as executable
```bash
/usr
chmod +x post-update
```

- Merge the `feature` branch into `master` branch
- Push the changes to remote repository
```bash
cd /usr/src/kodekloud/cluster/

git merge master feature

git push
```

## Brief Description About the Environment

Git hooks are scripts Git runs automatically at specific points in a repository’s lifecycle to let you automate or enforce policies.

Key points (brief):
- Where: hooks live in the repository’s .git/hooks directory for non-bare repos, or in the hooks directory of a bare repo (e.g., /opt/cluster.git/hooks).  
- Types: two main groups
  - Client-side hooks — run on a developer’s machine (examples: pre-commit, prepare-commit-msg, commit-msg, post-commit, pre-push).
  - Server-side hooks — run on the Git server (examples: pre-receive, update, post-receive, post-update).
- Triggering: each hook name corresponds to a Git event; Git runs the script (if executable) when that event occurs.
- Input: some hooks receive arguments (e.g., update gets ref name, old SHA, new SHA), others read from stdin or environment.
- Exit codes: non-zero exit prevents the associated action (e.g., pre-commit failing stops the commit; pre-receive/update failing blocks the push). post-* hooks run after the action so their failures don’t block it.
- Use cases:
  - Enforce policies (commit message format, code style).
  - Run tests or linters before allowing pushes/commits.
  - Automatically create tags, deploy, notify systems, or update CI when pushes occur.
- Format: any executable file (bash, python, etc.). Make it executable: chmod +x .git/hooks/<hook-name>.
- Security: hooks are local files and are not versioned by default; if you want shared hooks, store them in the repo and provide an installer or use core.hooksPath to point to a shared directory.
- Best practices:
  - Fail fast in pre-* hooks to prevent bad changes.
  - Keep hooks fast and deterministic.
  - Log output for server hooks for debugging.
  - Use annotated tags and check for existing tags when creating them in hooks.

Script code form [fresh-joey](https://github.com/fresh-joey/100-days-of-devops-by-kodekloud/blob/main/Day034.md)
