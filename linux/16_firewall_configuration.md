# Firewall Configuration

## Question

The Nautilus system admins team has rolled out a web UI application for their backup utility on the Nautilus backup server within the Stratos Datacenter. This application operates on port 5001, and firewalld is active on the server. To meet operational needs, the following requirements have been identified:

Allow all incoming connections on port 5001/tcp. Ensure the zone is set to public.

## Answer

- SSH into Backup Server
- Ensure firewalld is running
```bash
ssh clint@stbkp01

sudo su -

firewall-cmd -state
```

- Add the rule and reload firewalld service
```bash
firewall-cmd --add-port=5001/tcp --zone=public --permanent

firewall-cmd --reload
```

## Brief Description About the Environment

firewalld is a firewall management tool for Linux operating systems. It provides firewall features by acting as a front-end for the Linux kernel's netfilter framework. firewalld's current default backend is nftables. 
