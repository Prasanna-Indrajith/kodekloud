# SElinux Installation and Configuration

## Question

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 1 in the Stratos Datacenter:

Install the required SELinux packages.

Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

## Answer

- Get the Secure Shell Access to App Server 01
```bash
sudo ssh tony@stapp01

sudo su -
```

- Install SElinux and necessary packages (Use `dnf`. When use `yum` there are some problem occurs.)
```bash
sudo dnf install -y selinux-policy-targeted policycoreutils policycoreutils-python-utils
```

- Edit the configuration file to put it into `disabled` mode
```bash
vi /etc/selinux/config
```

- In config file change this
```bash
SELINUX=disabled
```
