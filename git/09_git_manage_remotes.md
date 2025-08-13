# git manage remotes

## Question

The xFusionCorp development team added updates to the project that is maintained under /opt/beta.git repo and cloned under /usr/src/kodekloudrepos/beta. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/beta repository as per details mentioned below:

a. In /usr/src/kodekloudrepos/beta repo add a new remote dev_beta and point it to /opt/xfusioncorp_beta.git repository.

b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch.

c. Finally push master branch to this new remote origin.

## Answer

Here is the process
```bash
ssh natasha@ststor01

cd /usr/src/kodekloudrepos/beta

sudo su -

git remote add dev_beta /opt/xfusioncorp_beta.git

cp /tmp/index.html .

git add index.html

git commit -m "index.html added after add new remote"

git push /opt/xfusioncorp_beta.git master
```

## Additional Details

git remote add
```bash
git remote add <new-remote-name> <link/remote-location>
```

git push
```bash
git push <remote-repo-location/link> <branch>
```

`git remote add` adds a new remote repository reference to your local Git project. This allows you to interact (push, pull, fetch) with another repository, typically hosted on a server like GitHub or GitLab. For example, `git remote add origin <url>` sets up a remote named "origin" pointing to the specified URL.

To push into a remote repo, you use `git push <remote> <branch>`. This uploads your local branch commits to the remote repository. For example, `git push origin main` sends your local `main` branch changes to the `origin` remote. You must have write access to the remote repository.
