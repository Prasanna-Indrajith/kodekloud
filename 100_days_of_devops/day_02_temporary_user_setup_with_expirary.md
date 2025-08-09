# Temporary User Setup with Expiry

## Question

As part of the temporary assignment to the Nautilus project, a developer named john requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

Create a user named john on App Server 2 in Stratos Datacenter. Set the expiry date to 2024-04-15, ensuring the user is created in lowercase as per standard protocol.

## Answer

ssh into App Server 02
```bash
ssh steve@172.16.238.11 > Am3ric@
```

create user with expiry day
```bash
sudo adduser -e 2024-04-12 john
```

is that user created ?
```bash
cat /etc/passwd
```

```bash
grep john /etc/passwd
```

```bash
sudo chage -l john
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

## Additional Details

## Brief Description About the Environment

**Linux User Account Expiration & Temporary Access Management**

User account expiration is a critical security feature in Linux systems that automatically disables accounts after a specified date. This is particularly important for:

- **Temporary contractors**: Developers or consultants with time-limited project access
- **Compliance requirements**: Meeting security policies that mandate access review cycles
- **Project-based access**: Users who only need access during specific project phases
- **Automated security**: Reducing manual overhead of remembering to disable accounts

**Key Components:**

**Account Expiration vs Password Expiration:**
- **Account expiration** (`-e` flag): Completely disables the account after the specified date
- **Password expiration**: Only forces password changes, account remains active

**The `chage` Command:**
- `chage -l username`: Lists account aging information including expiration dates
- `chage -E YYYY-MM-DD username`: Sets account expiration date
- `chage -M days username`: Sets maximum password age
- `chage -d 0 username`: Forces password change on next login

**Account Information Storage:**
- `/etc/passwd`: Contains basic user information (username, UID, GID, home directory, shell)
- `/etc/shadow`: Stores password hashes, last password change, expiration data, and account expiration dates
- Shadow file format includes expiration date as days since Jan 1, 1970

**Verification Methods:**
- `grep username /etc/passwd`: Quick check if user exists
- `chage -l username`: Detailed account aging information
- `cat /etc/shadow | grep username`: Raw shadow file entry (requires root access)
- `su - username`: Test account login functionality

**Nautilus Project Context:**
The script references the "Nautilus project" which appears to be a temporary initiative requiring controlled access management across the Stratos Datacenter infrastructure, emphasizing the importance of time-limited access controls in enterprise environments.
