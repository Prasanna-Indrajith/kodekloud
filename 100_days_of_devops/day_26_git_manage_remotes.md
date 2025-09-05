# Git Manage Remotes

## Question

The xFusionCorp development team added updates to the project that is maintained under /opt/news.git repo and cloned under /usr/src/kodekloudrepos/news. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/news repository as per details mentioned below:

a. In /usr/src/kodekloudrepos/news repo add a new remote dev_news and point it to /opt/xfusioncorp_news.git repository.

b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch.
c. Finally push master branch to this new remote origin.

## Answer

- Log into Storage Server and get the `sudo` access.
- Change the directory into /usr/src/kodekloudrepos/news
```bash
ssh natasha@ststor01

sudo su -

cd /usr/src/kodekloudrepos/news
```

- Add new remote
- Copy `index.html` file
```bash
git add remote add dev_news /opt/xfusioncorp_news.git

cp /tmp/index.html
```

- Do the rest of normal stuffs
```bash
git add index.html

git commit -m "New index.html added!"

git push dev_news master
```

## Additional Details

We can check current remotes
```bash
git remote
```

## Brief Description About the Environment

To manage remotes in Git, you can use commands like `git remote add <name> <url>` to add a new remote, `git remote remove <name>` to delete a remote, and `git remote set-url <name> <new-url>` to change an existing remote's URL. You can also list your current remotes with `git remote -v` to see their names and associated URLs.
