# Secure ssh root access

## Question

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.

## Answer

SSH into 3 app server with 3 termials
```bash
ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03
```

Get the super user previlages
```bash
sudo sh -
```

Edit the `PermitRootLogin yes` in `/etc/ssh/sshd_config`
```bash
vi /etc/ssh/sshd_config

/PermitRootLogin no
```

Restart sshd service
```bash
systemctl restart sshd
```

## Brief Description About the Environment

### Why Direct Root Login is a Security Risk

* **Brute-Force Attacks**: The username 'root' is universally known. This makes it a primary target for attackers attempting to guess the password through automated scripts.
* **No Accountability**: Direct root login bypasses user-specific logging. When a standard user logs in and then uses `sudo` to gain root privileges, the system logs which specific user performed the action. With direct root login, all actions are logged as being performed by 'root', making it difficult to trace who did what.
* **Potential for Catastrophic Errors**: The root user has unrestricted access to the entire system. A single mistake, such as an incorrect command, could lead to accidental deletion of critical system files and render the system unusable.

***

### Best Practices

Instead of direct root login, the recommended best practice is to:

1.  **Disable root login:** Edit the SSH configuration file (`/etc/ssh/sshd_config`) and set `PermitRootLogin` to `no`.
2.  **Use a standard user account:** Create a regular user account for remote access.
3.  **Use `sudo` for elevated privileges:** When you need to perform administrative tasks, log in with the standard user account and use the `sudo` command to temporarily elevate your privileges. This provides a balance of security and administrative convenience.udo command to temporarily elevate your privileges. This provides a balance of security and administrative convenience.
