# IPtables Installation and Configuration

## Question

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 5000 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

1. Install iptables and all its dependencies on each app host.

2. Block incoming port 5000 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.

## Answer

SSH into each app server and do the same thing down below.
```bash
ssh tony@stapp01

sudo su -

# Install iptables
yum install iptables-services -y

# Enable iptables
systemctl enable iptables

systemctl start iptables

# Add new ALLOW(Accept) rule on `Nautilus HTTP LBR` in iptables
iptables -I INPUT 1 -p tcp --dport 5000 -s 172.16.238.14 -j ACCEPT

# Block all the incomming connections from port 5000(accept only LBR)
iptables -I INPUT -p tcp --dport 5000 -j DROP

# View the iptable to ensure our rules are added or not.
iptables -L -n

# Save the iptables new rules configuration
iptables-save > /etc/sysconfig/iptables
```

## Additional Commands

```bash
telnet stapp01 5000
```

## Brief Description About the Environment

**iptables for what ?**

iptables is used on Linux systems to manage and configure the firewall. It controls network traffic by defining rules for allowing, blocking, or redirecting packets based on IP addresses, ports, protocols, and other criteria. This helps secure servers and networks from unauthorized access and attacks.

**different between iptable and User firewall (ufw)**

iptables is a low-level firewall utility that allows fine-grained control over network traffic rules on Linux. It requires manual rule configuration and has a complex syntax.

UFW (Uncomplicated Firewall) is a user-friendly frontend for iptables. It simplifies firewall management with easy commands and default policies, making it suitable for beginners or quick setups.

In summary:
- iptables: Powerful, flexible, complex, manual configuration.
- UFW: Simplified, user-friendly, uses iptables underneath.
