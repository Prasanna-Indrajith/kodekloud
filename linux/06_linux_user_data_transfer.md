# Linux user data transfer

## Question
Due to an accidental data mix-up, user data was unintentionally mingled on Nautilus App Server 3 at the /home/usersdata location by the Nautilus production support team in Stratos DC. To rectify this, specific user data needs to be filtered and relocated. Here are the details:

Locate all files (excluding directories) owned by user ammar within the /home/usersdata directory on App Server 3. Copy these files while preserving the directory structure to the /blog directory.

## Answer

SSH into App Server 3 - stapp03 - banner
```bash
ssh banner@stapp03
```

Find all the files owned by user ammer
```bash
find /home/usersdata/ -type f -user ammar
```

Copy all the files (ONLY files -type f) in this /home/usersdata directory to destination (/blog)
```bash
find /home/usersdata/ -type f -user ammar -exec cp --parents {} /blog \;
```

## Additional Details

This command copies all files owned by the user `ammar` from `/home/usersdata/` to `/blog`, preserving their directory structure. Here’s the breakdown:

- `find /home/usersdata/ -type f -user ammar`: Finds all files (`-type f`) in `/home/usersdata/` owned by user `ammar`.
- `-exec cp --parents {} /blog \;`: For each found file (`{}`), runs `cp --parents` to copy the file to `/blog`, preserving the original directory structure relative to `/home/usersdata/`.

**Result:**
All files owned by `ammar` are copied to `/blog`, keeping their folder hierarchy intact.

## Brief Description About the Environment

**Find and copy data owned by user**

The find command in Linux is a powerful utility used to search for files and directories within a directory hierarchy based on various criteria such as name, type, size, modification time, and permissions. It recursively traverses directories starting from a specified path and can filter results using options like -name, -type, -mtime, and more. This makes it highly flexible for locating files that match complex conditions, making it a staple tool for system administrators and power users.

The -exec option with find allows you to execute a specified command on each file or directory found by the search. For example, using find with -exec cp {} /destination \; will copy each found file to the /destination directory, where {} is replaced by the current file path. The command is terminated with \; to indicate the end of the exec statement. This combination is especially useful for automating batch operations, such as copying, moving, or deleting files that meet certain criteria, all in a single command line.
