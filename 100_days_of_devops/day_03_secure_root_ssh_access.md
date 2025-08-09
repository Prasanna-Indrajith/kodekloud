# Secure Root SSH Access

## Question

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.

## Answer

login to each server
```bash
ssh banner@172.16.238.12 -> BigGr33n
```

To achieve that thing we need to turn on a feature in ssh config file
```bash
nano /etc/ssh/sshd_config
```

uncomment line
```bash
# #PermitRootLogin no
PermitRootLogin no
```

restart sshd
```bash
systemctl restart sshd
```

```bash
systemctl status sshd
```

## Additional Details

## Brief Description About the Environment

**SSH Security Hardening & Root Access Control**

SSH (Secure Shell) is the primary remote access protocol for Linux systems, making its security configuration critical for infrastructure protection. Disabling direct root SSH login is a fundamental security practice that addresses several key vulnerabilities:

**Security Benefits:**
- **Eliminates single-point-of-failure**: Prevents attackers from directly targeting the most privileged account
- **Enforces accountability**: Requires users to authenticate with personal accounts first, creating audit trails
- **Implements privilege escalation**: Forces users to use `sudo` or `su`, adding an extra authentication layer
- **Reduces brute force attack surface**: Eliminates automated attacks targeting the known "root" username

**SSH Configuration File (`/etc/ssh/sshd_config`):**

This is the main configuration file for the SSH daemon that controls how SSH connections are handled. Key security directives include:

- `PermitRootLogin no`: Disables direct root login completely
- `PermitRootLogin without-password`: Allows root login only with SSH keys (no passwords)
- `PermitRootLogin forced-commands-only`: Restricts root to specific commands only
- `AllowUsers`: Whitelist specific users who can SSH
- `DenyUsers`: Blacklist users from SSH access
- `Port`: Change default SSH port from 22
- `PasswordAuthentication`: Control password-based authentication

**Service Management:**
- `systemctl restart sshd`: Applies configuration changes by restarting the SSH daemon
- `systemctl status sshd`: Verifies the service is running and shows recent log entries
- `systemctl reload sshd`: Reloads configuration without dropping existing connections

**Best Practices After Disabling Root SSH:**
1. **Ensure sudo access**: Verify that administrative users have proper sudo privileges
2. **Test access**: Confirm that legitimate administrators can still access the system
3. **Document changes**: Record configuration modifications for compliance and troubleshooting
4. **Monitor logs**: Watch `/var/log/secure` or `/var/log/auth.log` for failed login attempts

**Enterprise Context:**
The xFusionCorp Industries security audit represents a common scenario where organizations must implement security hardening across their entire infrastructure. The Stratos Datacenter likely contains multiple application servers that require consistent security configurations to maintain compliance and reduce attack vectors.
