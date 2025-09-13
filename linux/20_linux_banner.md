# Linux Banner

## Question

During the monthly compliance meeting, it was pointed out that several servers in the Stratos DC do not have a valid banner. The security team has provided serveral approved templates which should be applied to the servers to maintain compliance. These will be displayed to the user upon a successful login.

Update the message of the day on all application and db servers for Nautilus. Make use of the approved template located at /home/thor/nautilus_banner on jump host

## Answer

- I know this is hard way!
- Securely copy the `nautilus_banner` file into all the application servers and Database Server
```bash
scp nautilus_banner tony@stapp01:~/
```

- Login to each server and copy the content of `nautilus_banner` to `/etc/motd`
```bash
sudo echo nautilus_banner >> /etc/motd
```

## Brief Description About the Environment

**Linux Banner**

1. anner message to display before user logs in (configure in the file of your choice eg. /etc/login.warn)

2. Banner message to display after the user successfully logged in (configure in /etc/motd)
