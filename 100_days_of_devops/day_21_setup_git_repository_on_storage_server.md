# Set Up Git Repository on Storage Server

## Question

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

Utilize yum to install the git package on the Storage Server.
Create a bare repository named /opt/news.git (ensure exact name usage).

## Answer

- SSH into Storage Server
```bash
ssh natasha@ststor01
```

- install git
```bash
sudo yum install git -y
```

- make directory
```bash
sudo su -

cd /opt
mkdir news.git
cd news.git
```

- initialize repository as `--bare`
```bash
git ini --bare

ls
```

## Brief Description About the Environment

**What is `--bare`**

Think of it as a Git database without the checkout capability. When you look inside a bare repository, you’ll notice it doesn’t have the typical project files you can edit. Instead, you’re seeing the actual repository storage structure that’s normally hidden in the .git directory of regular repositories.

Bare repositories follow specific naming conventions. They typically end with a .git suffix, signaling their nature as storage-only repositories. This convention helps distinguish them from regular repositories at a glance.

- When we do `git push` We push our files into `remote repository`, which is hold our files. So that's it. **`git init --bare` with `.git` extention folder.** We can initialize remote repository. which can hold our `push`es. That's why it's in storage server.

regular-repo/            bare-repo.git/
├── .git/                ├── HEAD
│   ├── HEAD             ├── config
│   ├── config           ├── description
│   ├── description      ├── hooks/
│   ├── hooks/           ├── info/
│   ├── info/            ├── objects/
│   ├── objects/         ├── refs/
│   └── refs/            └── packed-refs
└── working files

https://tms-outsource.com/blog/posts/what-is-a-bare-repository/
