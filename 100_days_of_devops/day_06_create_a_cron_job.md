# Create a Cron Job

## Question

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

Install cronie package on all Nautilus app servers and start crond service. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.

## Answer

login to 3 stapps with 03 terminal windows
```bash
ssh tony@stapp01
  > Ir0nM@n
```

```bash
ssh steve@stapp02
  > Am3ric@
```

```bash
ssh banner@stapp03
  > Biggr33n
```

repeat this below process on each server

install cronie
```bash
yum install cronie
```

start the crond service
```bash
systemctl start crond.service
```

show status
```bash
systemctl status crond
```

list crontab root user tasks
```bash
sudo crontab -u root -l
  -> nothing yet
```

Add new job to root - e (edit)
```bash
sudo crontab -e
  -> */5 * * * * echo hello > /tmp/cron_text
  -> :wq
```

view that job is assigned to user
```bash
sudo crontab -u root -l
```

## Additional Details

## Brief Description About the Environment

**Cron Job Scheduling & Task Automation**

Cron is a time-based job scheduler in Unix-like operating systems that enables automatic execution of commands, scripts, and programs at specified intervals. It's essential for system administration, maintenance tasks, and automated processes in enterprise environments.

**Core Components:**

**Cron Daemon (crond):**
- Background service that runs continuously and checks for scheduled tasks
- Reads cron tables (crontabs) every minute to determine what jobs to execute
- Logs execution details for monitoring and troubleshooting
- Requires the `cronie` package on RHEL/CentOS systems

**Crontab Format:**
```
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of week (0-7, Sunday = 0 or 7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
```

**Schedule Examples:**
- `*/5 * * * *`: Every 5 minutes
- `0 2 * * *`: Daily at 2:00 AM
- `0 0 1 * *`: First day of every month at midnight
- `30 9 * * 1-5`: 9:30 AM, Monday through Friday
- `0 */4 * * *`: Every 4 hours

**Crontab Management Commands:**
- `crontab -l`: List current user's cron jobs
- `crontab -e`: Edit current user's crontab
- `crontab -r`: Remove current user's crontab
- `crontab -u username -l`: List specific user's cron jobs (requires root)
- `crontab -u username -e`: Edit specific user's crontab (requires root)

**System vs User Crontabs:**
- **User crontabs**: Individual schedules stored in `/var/spool/cron/username`
- **System crontabs**: Located in `/etc/crontab` and `/etc/cron.d/`
- **Predefined directories**: `/etc/cron.hourly/`, `/etc/cron.daily/`, `/etc/cron.weekly/`, `/etc/cron.monthly/`

**Enterprise Deployment Considerations:**

**Multi-Server Coordination:**
The script demonstrates deploying identical cron jobs across multiple servers (stapp01, stapp02, stapp03), which is common for:
- Load-balanced environments requiring synchronized tasks
- Backup operations across multiple nodes
- Log rotation and cleanup processes
- Health checks and monitoring scripts

**Security Best Practices:**
- **Root crontabs**: Used for system-level tasks but require careful management
- **Output redirection**: Commands should redirect output to avoid email spam to root
- **Path considerations**: Always use absolute paths in cron commands
- **Environment variables**: Cron runs with minimal environment, so set PATH, HOME, etc. as needed

**Logging and Monitoring:**
- Cron logs are typically found in `/var/log/cron`
- Failed jobs may send email to the user account
- Use `> /dev/null 2>&1` to suppress output if not needed
- Consider logging important cron job results to custom log files

**Testing Strategy:**
The Nautilus team's approach of starting with a simple test cron job (`echo hello > /tmp/cron_text`) is a best practice that allows verification of:
- Cron service functionality
- File system permissions
- Job scheduling accuracy
- Multi-server deployment consistency

**Troubleshooting Common Issues:**
- Service not running: `systemctl status crond`
- Permission issues: Check file ownership and SELinux contexts
- Environment problems: Test commands manually first
- Timing issues: Verify system time synchronization across servers

This foundation enables the Nautilus team to confidently deploy more complex automation scripts across their Stratos Datacenter infrastructure.
