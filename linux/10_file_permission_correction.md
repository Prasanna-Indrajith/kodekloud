# File permission correction

## Question

After conducting a security audit within the Stratos DC, the Nautilus security team discovered misconfigured permissions on critical files. To address this, corrective actions are being taken by the production support team. Specifically, the file named /etc/hosts on Nautilus App 2 server requires adjustments to its Access Control Lists (ACLs) as follows:

1. The file's user owner and group owner should be set to root.

2. Others should possess read only permissions on the file.

3. User john must not have any permissions on the file.

4. User ryan should be granted read only permission on the file.

## Answer

SSH into app server 02 : steve and get super user privelages
```bash
ssh steve@stapp02

sudo su -
```

Give user and group permissions to `root`
```bash
chown root:root /etc/hosts
```

Read only permission for groups and Others
```bash
chmod 644 /etc/hosts
```

John no any permission on this file, and ryan has only read permission
```bash
setfacl -m u:ryan:r-- /etc/hosts

setfacl -m u:john:--- /etc/hosts
```

Check all the file permission Details
```bash
getfacl /etc/hosts
```

## Brief Description About the Environment

`setfacl` and `getfacl` are commands used in Linux to manage **Access Control Lists (ACLs)**, which provide a more granular way to set file permissions than the standard `chmod` command. While `chmod` restricts permissions to the owner, group, and "others," ACLs allow you to define permissions for specific individual users or groups.

---

### `getfacl`

The `getfacl` command is used to **get** or display the ACLs for a file or directory. When you run `getfacl` on a file, it shows you a detailed breakdown of its permissions. This output includes:

* **File name, owner, and group:** This is the basic information you'd get from `ls -l`.
* **User permissions:** Shows permissions for the file owner and any specific users you've added.
* **Group permissions:** Shows permissions for the owning group and any specific groups you've added.
* **Mask:** This is a special entry that defines the maximum permissions that can be granted to all users (except the owner) and groups that have an ACL entry.
* **Other permissions:** The permissions for all other users on the system.

This command is essential for verifying that permissions have been set correctly and for troubleshooting access issues.

---

### `setfacl`

The `setfacl` command is used to **set**, add, modify, or delete ACL entries on files and directories. It's the primary tool for implementing fine-grained permissions. Here are some of its common options:

* **`-m` (modify):** This is used to add or modify an ACL entry. For example, `setfacl -m u:john:rwx file.txt` grants user `john` read, write, and execute permissions on `file.txt`.
* **`-x` (remove):** This option removes a specific ACL entry. For example, `setfacl -x u:john file.txt` removes `john`'s ACL entry from `file.txt`.
* **`-b` (remove all):** This option removes all extended ACL entries, returning the permissions to the standard owner, group, and others model.
* **`-R` (recursive):** This applies the changes to a directory and all of its contents.
* **`--set`:** This replaces the entire existing ACL with a new one you specify.

The `setfacl` command gives administrators precise control over who can access a file, which is a key component of a robust security posture.

You can learn more about how to use these commands with a hands-on tutorial. [Linux ACL permissions (getfacl / setfacl) explained](https://www.youtube.com/watch?v=J-MziEKPq3A) is a video that explains how ACL permissions work using `getfacl` and `setfacl`.
http://googleusercontent.com/youtube_content/0

