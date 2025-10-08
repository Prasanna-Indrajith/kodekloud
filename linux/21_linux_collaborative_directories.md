# Linux collaborative directories

## Question

The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the `sysadmin` group of the team.

Setup a collaborative directory `/sysadmin/data` on `app server 3` in Stratos Datacenter.

The directory should be group owned by the group `sysadmin` and the group should own the files inside the directory. The directory should be `read/write/execute` to the **user and group owners**, and others should not have any access.

## Answer

- Login to app server 03 and make correct directories

```bash
ssh banner@stapp03
sudo su

mkdir /sysadmin
mkdir /sysadmin/root
```

- Check whether `sysadmin` group is exits

```bash
cat /etc/group | grep sysadmin
```

- Set group ownership to `sysadmin`

```bash
chgrp sysadmin /sysadmin/data
```

- Set permissions (read/write/execute for user and group, none for others)

```bash
chmod 770 /sysadmin/data
```

- Enable setgid so new files inherit the group

```bash
chmod g+s /sysadmin/data
```
