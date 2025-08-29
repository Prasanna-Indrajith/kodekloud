# Default GUI Boot Configuration

## Question

With the installation of new tools on the app servers within the Stratos Datacenter, certain functionalities now necessitate graphical user interface (GUI) access.

Adjust the default runlevel on all App servers in Stratos Datacenter to enable GUI booting by default. It's imperative not to initiate a server reboot after completing this task.

## Answer

- log into each App Server and do the same.
```bash
systemctl get-default

sudo systemctl set-default graphical.target

systemctl get-default
```

## Brief Description About the Environment

The default runlevel in Linux typically refers to the state the system boots into, which is usually set to runlevel 3 (multi-user mode with networking) or runlevel 5 (multi-user mode with a graphical interface). This setting can be found in the `/etc/inittab` file on traditional SysV init systems.
