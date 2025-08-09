# Create user without home directory

## Question

In response to the latest tool implementation at xFusionCorp Industries, the system admins require the creation of a service user account. Here are the specifics:

Create a user named james in App Server 3 without a home directory.

## Answer

login to stapp03
```bash
ssh banner@stapp03
    > BigGr33n
```

Create user wihout home directory
```bash
sudo useradd -M james
```

exit from the stapp03
```bash
exit
```

## Additional Details

Additional commands with `useradd -M`

-M, --no-create-home
           Do not create the user's home directory, even if the system wide setting
           from /etc/login.defs (CREATE_HOME) is set to yes.


## Brief Description About the Environment

**User add without home**

The command sudo useradd -M james creates a new user named james on a Linux system without creating a home directory for that user. The -M option tells useradd not to create the user's home directory (usually /home/james). This is useful for system or service accounts that do not require a home directory.

