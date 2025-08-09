# Script Execution Permission

## Question

In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 2 within the Stratos Datacenter.

Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 2. Additionally, ensure that all users have the capability to execute it.

## Answer

login to stapp2
```bash
ssh steve@stapp2
-> Am3ric@
```

mod the permissions
```bash
sudo chmod 777 /tmp/xfusioncorp.sh
```

view permission
```bash
ls -lrt /tmp/xfusioncorp.sh
```

```bash
exit
```

## Additional Details

user group others
```
 7     7     7
 rwx  rwx   rwx

r (read) = 4
w (write) = 2
x (execute) = 1
– (no permission) = 0
```

## Brief Description About the Environment

**Linux File Permissions & Script Execution**

File permissions in Linux control who can access files and what operations they can perform. This three-tier permission system is fundamental to system security and proper script deployment across enterprise environments.

**Permission Structure:**
Linux uses a three-category permission model:
- **User (Owner)**: The file creator or assigned owner
- **Group**: Members of the file's assigned group
- **Others**: All other users on the system

**Permission Types:**
Each category can have three types of permissions:
- **Read (r/4)**: View file contents or list directory contents
- **Write (w/2)**: Modify file contents or create/delete files in directories
- **Execute (x/1)**: Run files as programs or access directories

**Octal Notation:**
The `chmod 777` command uses octal (base-8) notation where each digit represents the sum of permission values:
- `7 = 4+2+1` = read + write + execute (full permissions)
- `6 = 4+2` = read + write
- `5 = 4+1` = read + execute
- `4 = 4` = read only
- `0 = 0` = no permissions

**Common Permission Combinations:**
- `755`: Owner has full access, group and others can read and execute
- `644`: Owner can read/write, group and others can only read
- `700`: Only owner has any access (private files)
- `777`: Full permissions for everyone (use with caution)

**Script Deployment Considerations:**
- **Security**: While `777` grants maximum accessibility, it also poses security risks in production
- **Automation**: Scripts need execute permissions to run via cron jobs or automated systems
- **Backup processes**: Automated backup scripts typically need to read multiple files and directories
- **Multi-user access**: Enterprise environments often require scripts accessible to multiple system users

**Alternative Approaches:**
Instead of `777`, consider more secure options:
- `755`: Allows execution by all users but prevents modification by non-owners
- `750`: Restricts access to owner and group members only
- Setting specific group ownership with `chgrp` for controlled access

**Verification Commands:**
- `ls -l filename`: Shows detailed permissions in symbolic format (rwxrwxrwx)
- `ls -la`: Shows all files including hidden ones with permissions
- `stat filename`: Displays comprehensive file information including octal permissions

The `/tmp` directory location suggests this is a temporary deployment location, which is common for testing scripts before moving them to permanent locations like `/usr/local/bin` or `/opt/scripts`.
