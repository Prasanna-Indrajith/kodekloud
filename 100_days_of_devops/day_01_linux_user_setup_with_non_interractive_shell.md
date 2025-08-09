# Linux User Setup with Non-Interactive Shell

## Question

To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task:

Create a user named kareem with a non-interactive shell on App Server 3.

## Answer

ssh into App Server 03
```bash
ssh banner@172.16.238.12 > BigGr33n
```

here is the actual code
```bash
sudo useradd -s /sbin/nologin kareem  # -s (--shell)
```

## Additional Details

This is basic code
```bash
sudo adduser kareem
sudo userdel -r kareem
```

This is for show users
```bash
cat /etc/passwd
grep kareem /etc/passwd
```

see user ID
```bash
id kareem
```

see id and group
```bash
id -gn kareem
```

login to that added user
```bash
su - kareem # (-) for login directory (/home/kareem)
su kareem # this starts with current users home folder (/home/prasa)
```

show password - file
```bash
cat /etc/shadow
```

## Brief Description About the Environment

**Linux User Management & Non-Interactive Shells**

In Linux systems, user accounts can be configured with different types of shells depending on their intended purpose. A **non-interactive shell** like `/sbin/nologin` is specifically designed for system accounts that should not allow interactive login sessions. This is commonly used for:

- **Service accounts**: Users that run system services or applications
- **Backup agents**: Automated tools that need file system access but shouldn't allow human login
- **Security compliance**: Preventing unauthorized interactive access while maintaining system functionality

The `/sbin/nologin` shell immediately terminates any login attempt and can display a custom message. This provides better security than simply disabling the account, as it allows the user to exist for file ownership and process execution while preventing shell access.

**Key Linux User Management Files:**
- `/etc/passwd`: Contains user account information including usernames, UIDs, home directories, and default shells
- `/etc/shadow`: Stores encrypted passwords and account aging information
- `/etc/group`: Contains group membership information

**User Management Commands:**
- `useradd`: Add new user accounts with specific parameters
- `usermod`: Modify existing user accounts
- `userdel`: Remove user accounts and optionally their home directories
- `id`: Display user and group IDs
- `su`: Switch user context for testing access
