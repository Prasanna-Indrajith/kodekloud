# Restrict Cron Access

## Question

In alignment with compliance standards, the Nautilus project team has opted to impose restrictions on crontab access. Specifically, only designated users will be permitted to create or update cron jobs.

Configure crontab access on App Server 3 as follows: Allow crontab access to anita user while denying access to the jerome user.

## Answer

- ssh into App Server 3
```bash
ssh banner@stapp03

sudo su -
```

- To give access we need to ensure whether we have these two files inside `/etc/` folder
  - `cron.allow`
  - `cron.deny`

- If not create the two files and enter names of users according to allowance or denying.
```bash
cd /etc/

echo "anita" >> cron.allow
echo "jerome" >> cron.deny
```

## Brief Description About the Environment

**Cron?**

Cron is a built-in Linux utility that schedules tasks to run automatically at specified intervals, making it ideal for automating repetitive tasks like backups or system maintenance. Users define these scheduled tasks, known as cron jobs, in a configuration file called crontab.

Overview of Cron in Linux
Cron is a job scheduler in Unix-like operating systems, including Linux. It automates the execution of tasks at specified intervals or times without user intervention. This is particularly useful for repetitive tasks such as backups, system maintenance, and running scripts.

How Cron Works
Cron operates using a daemon called crond, which runs in the background. It reads configuration files known as crontabs, which contain the scheduled tasks. Each user can have their own crontab file, and there is also a system-wide crontab file.

Crontab Syntax
A crontab entry consists of five time-and-date fields followed by the command to execute:

- Field	Description
- Minute	0-59
- Hour	0-23
- Day of Month	1-31
- Month	1-12 (or Jan-Dec)
- Day of Week	0-6 (0 = Sunday)
- For example, the entry 30 1 * * * /path/to/script.sh runs the script at 1:30 AM every day.

Managing Cron Jobs
Users can manage their cron jobs using the crontab command:

`crontab -e`: Edit the current user's crontab.
`crontab -l`: List the current user's cron jobs.
`crontab -r`: Remove the current user's crontab.

**Special Features**

Some cron implementations support additional features, such as:

@reboot: Runs a job once at system startup.
Non-standard characters like L for "last" day of the month or W for the nearest weekday.
Cron is a powerful tool for automating tasks, making it essential for system administrators and users who need to schedule regular operations.
