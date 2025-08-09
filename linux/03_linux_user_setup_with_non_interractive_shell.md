# Linux User Setup with Non-Interactive Shell

## Question

The System admin team of xFusionCorp Industries has installed a backup agent tool on all app servers. As per the tool's requirements they need to create a user with a non-interactive shell. Therefore, create a user named mark with a non-interactive shell in the app02 server.

## Answer

ssh into App server 02
```bash
ssh steve@172.16.238.11 -> Am3ric@
```

add user without shell access
```bash
adduser -s /sbin/nologin mark
```

## Additional Details

## Brief Description About the Environment

**Non-Interactive Shell Users & Service Accounts**

Non-interactive shell users are system accounts designed to run services, applications, or automated processes without allowing human login sessions. This security practice is essential for service isolation, privilege separation, and minimizing attack surfaces in enterprise environments.

**Core Concepts:**

**Shell Types:**
- **Interactive Shells**: Allow human users to execute commands and interact with the system (`/bin/bash`, `/bin/zsh`)
- **Non-Interactive Shells**: Prevent login sessions while allowing process execution (`/sbin/nologin`, `/bin/false`)
- **Service Shells**: Specialized shells for system services and daemons
- **Restricted Shells**: Limited functionality shells for specific use cases

**Non-Interactive Shell Options:**
- `/sbin/nologin`: Displays a polite message and terminates login attempts
- `/bin/false`: Immediately returns exit status 1 and terminates
- `/dev/null`: Redirects to null device (less common)
- Custom scripts: Can provide specific behavior for login attempts

**Security Benefits:**

**Attack Surface Reduction:**
- **No Interactive Access**: Eliminates possibility of unauthorized shell access
- **Process Isolation**: Service runs under dedicated user without login capability
- **Privilege Separation**: Service permissions isolated from interactive user privileges
- **Audit Trail**: Clear separation between service actions and human user actions

**Service Account Best Practices:**
- **Dedicated Users**: Each service gets its own user account
- **Minimal Permissions**: Only permissions necessary for service operation
- **Home Directory Control**: Appropriate home directory for service needs
- **Group Membership**: Proper group assignments for required access

**Backup Agent Integration:**

**Backup Tool Requirements:**
- **File System Access**: Read access to files and directories being backed up
- **Process Execution**: Ability to run backup processes and scripts
- **Network Access**: Connect to backup servers or cloud storage
- **Scheduling Integration**: Work with cron jobs or system schedulers

**Why Non-Interactive Shell:**
- **Security Compliance**: Prevents unauthorized human access to backup user
- **Service Isolation**: Backup processes run independently of interactive users
- **Automated Operation**: Backup agent operates without human intervention
- **Audit Requirements**: Clear separation of service actions from user actions

**User Account Management:**

**User Creation Commands:**
- `adduser -s /sbin/nologin username`: Creates user with non-interactive shell
- `useradd -s /sbin/nologin username`: Alternative user creation command
- `adduser -r -s /sbin/nologin username`: Creates system user (UID < 1000)
- `adduser -M -s /sbin/nologin username`: Creates user without home directory

**Account Verification:**
```bash
# Check user exists and shell assignment
grep mark /etc/passwd
# Expected output: mark:x:UID:GID::/home/mark:/sbin/nologin

# Verify user information
id mark

# Test shell behavior
su - mark  # Should display message and exit
```

**Service Account Configuration:**

**File Permissions:**
- **Home Directory**: `/home/mark` or service-specific directory
- **Configuration Files**: Proper ownership and permissions for backup agent config
- **Log Files**: Writable directories for backup agent logs
- **Temporary Files**: Access to temporary storage as needed

**Process Management:**
- **Service Files**: Systemd service files specify User= directive
- **Cron Jobs**: Backup schedules run under service account
- **Process Monitoring**: Monitor backup agent processes under correct user
- **Resource Limits**: Set appropriate resource limits for backup processes

**Example Service Configuration:**
```ini
# /etc/systemd/system/backup-agent.service
[Unit]
Description=Backup Agent Service
After=network.target

[Service]
Type=forking
User=mark
Group=mark
ExecStart=/opt/backup-agent/bin/backup-daemon
PIDFile=/var/run/backup-agent.pid
Restart=always

[Install]
WantedBy=multi-user.target
```

**Enterprise Deployment Considerations:**

**Multi-Server Consistency:**
- **Uniform User Creation**: Same user account across all app servers (stapp01, stapp02, stapp03)
- **Synchronized UIDs**: Consistent user IDs for NFS or shared storage
- **Configuration Management**: Automated deployment using tools like Ansible or Puppet
- **Documentation**: Clear records of service account purposes and configurations

**Backup Agent Deployment:**
- **Installation Consistency**: Same backup agent version across all servers
- **Configuration Synchronization**: Consistent backup policies and schedules
- **Monitoring Integration**: Centralized monitoring of backup agent status
- **Log Aggregation**: Collect backup logs from all servers for analysis

**Security Hardening:**

**Access Control:**
- **Sudo Restrictions**: Limit or eliminate sudo access for service accounts
- **File System Permissions**: Restrict access to only necessary directories
- **Network Security**: Firewall rules for backup traffic
- **SELinux/AppArmor**: Additional mandatory access controls if enabled

**Monitoring and Auditing:**
- **Process Monitoring**: Track backup agent processes and resource usage
- **File Access Auditing**: Monitor what files the backup agent accesses
- **Network Traffic**: Monitor backup-related network connections
- **Log Analysis**: Regular review of backup agent logs for issues or security events

**Common Backup Agent Operations:**

**Typical Backup Tasks:**
- **File System Scanning**: Read file metadata and content
- **Incremental Backups**: Compare current state with previous backups
- **Compression**: Compress backup data before transmission
- **Encryption**: Encrypt backup data for security
- **Network Transfer**: Upload backup data to remote storage

**Required Permissions:**
- **Read Access**: Files and directories being backed up
- **Execute Access**: Backup agent binaries and scripts
- **Write Access**: Temporary directories and log files
- **Network Access**: Connections to backup servers or cloud services

**Troubleshooting Common Issues:**

**Account Problems:**
- **Shell Access Denied**: Verify `/sbin/nologin` is working correctly
- **Permission Errors**: Check file ownership and directory permissions
- **Service Won't Start**: Verify user account exists and has proper permissions
- **Backup Failures**: Check backup agent logs and file access permissions

**Verification Steps:**
```bash
# Confirm user creation
getent passwd mark

# Test login prevention
ssh mark@localhost  # Should fail with nologin message

# Check service can run as user
sudo -u mark /path/to/backup/agent --test

# Monitor backup process
ps aux | grep backup | grep mark
```

This implementation ensures that xFusionCorp Industries maintains secure service account practices while enabling their backup agent tool to operate effectively across their application server infrastructure.
