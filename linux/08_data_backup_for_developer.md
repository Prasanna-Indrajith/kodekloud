# Data backup for developer

## Question

Within the Stratos DC, the Nautilus storage server hosts a directory named /data, serving as a repository for various developers non-confidential data. Developer kirsty has requested a copy of their data stored in /data/kirsty. The System Admin team has provided the following steps to fulfill this request:

a. Create a compressed archive named kirsty.tar.gz of the /data/kirsty directory.

b. Transfer the archive to the /home directory on the Storage Server.

## Answer

SSh into storage server - ststore01 - natasha
```bash
ssh natasha@ststor01
```

Create compressed tar file and move it into /home directory
```bash
tar -czvf rose.tar.gz /data/rose

sudo mv rose.tar.gz /home
```

## Additional Details

Description
```bash
tar: The command itself.

-c: Create a new archive.

-z: Compress the archive using gzip.

-v: Verbose mode, which lists the files being added to the archive (this is optional but helpful).

-f: Specifies the filename of the archive.
```

## Brief Description About the Environment

**What is tar?**

TAR, which stands for **Tape Archive**, is a file archiving format and a command-line utility used in Unix-like operating systems. It's primarily designed to **bundle multiple files and directories into a single file**, or "tarball," for easier storage and distribution. While `tar` itself doesn't compress the files, it's often used with compression utilities like `gzip` or `bzip2` to create compressed archives, such as `.tar.gz` or `.tar.bz2`, which saves disk space and reduces transfer times. The `tar` command is a fundamental tool for system administrators and developers for tasks like creating backups, packaging software, and moving data between systems.
