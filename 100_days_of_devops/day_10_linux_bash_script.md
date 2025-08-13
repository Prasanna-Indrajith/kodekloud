# Linux Bash Script

## Question

The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named official_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 1).

a. Create a zip archive named xfusioncorp_official.zip of /var/www/html/official directory.

b. Save the archive in /backup/ on App Server 1. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

## Answer
ssh into app server 01 - tony
```bash
ssh tony@stapp01

```

get sudo previlages and cd into /scripts directory
```bash
sudo su -

cd /scripts

vi official_backup.sh
```

```official_backup.sh
#!/usr/bin/env bash

# Create a zip archive
zip -r xfusioncorp_official.zip /var/www/html/official

# mov zip into /backup
mv xfusioncorp_official.zip /backup/

# backup the file into Backup Server
scp /backup/xfusioncorp_official.zip clint@stbkp01:/backup/
```

Give executable permissions to normal user
```bash
chmod 755 ./official_backup.sh
```

Password less `scp`/ login to Backup Server 01
```bash
mkdir ~/.ssh/

ssh-keygen -t rsa

ssh-copy-id clint@stbkp01
```

## Additional Details

To run the script
```bash
cd /scripts

./official_backup.sh
```

