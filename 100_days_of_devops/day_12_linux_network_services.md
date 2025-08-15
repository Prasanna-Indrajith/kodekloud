# Linux network servicers

## Question

Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 5004 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command curl http://stapp01:5004 command from jump host.

## Answer

SSH and get sudo previlages into App Server 01 : tony
```bash
ssh tony@stapp01

sudo su -
```

Look what's going on with apache services. and try to enable and start and look is that problem gone by runnig `curl https://stapp02:5004`
```bash
systemctl status httpd

systemctl enable httpd

systemctl start httpd
```
 This throws an some error messages : I saw somethig says like "service can't start because another service is usig ..".

Looking for who is using `port:5004`
```bash
netstat -tupln | grep 5004

< OUTPUT
tcp        0      0 127.0.0.1:5004          0.0.0.0:* LISTEN      432/sendmail: accep
```
This means there is a program called `sendmail`, which currently use the port `5004`

We need to stop this service order to use this port for apache services.
```bash
sudo systemctl stop sendmail
```

List existing rules in **iptables**. Add a new rule: Insert a rule to allow inbound TCP traffic on port 5004. The `-I INPUT` option inserts the rule at the beginning of the INPUT chain, ensuring it's processed before any generic "deny all" rules.
```bash
iptables -L -n

iptables -I INPUT -p tcp --dport 5004 -j ACCEPT

service ipdatble save

service iptables reload
```

Now trying start the `httpd` service (apache service) and see whether our task is done or not.
```bash
sudo systemctl start httpd

curl https://stapp01:5004
```

## Brief Description About the Environment\

### `httpd`
The `httpd` command is the executable for the **Apache HTTP Server**. It is a daemon that runs in the background, handling web requests on the server. You use it with `systemctl` to manage the service, such as starting, stopping, or checking its status.

***

### `netstat`
`netstat` is a command-line tool for displaying **network connections, routing tables, and interface statistics**. It's useful for diagnosing network-related issues by showing which ports are open and which services are using them.

***

### `netstat -tulnp`
This is a specific combination of `netstat` options:
* **`-t`** shows **TCP** connections.
* **`-u`** shows **UDP** connections.
* **`-l`** shows **listening** sockets (ports waiting for a connection).
* **`-n`** shows numerical addresses instead of trying to resolve hostnames.
* **`-p`** shows the **PID** (Process ID) and name of the program that owns the socket. This is crucial for identifying which service is using a specific port.

***

### `iptables`
`iptables` is a firewall utility that manages **packet filtering rules** in the Linux kernel. It allows you to create rules that determine whether network packets are accepted, dropped, or rejected based on criteria like source, destination, and port.

***

### `iptables -L -n`
This command is used to **list the firewall rules**.
* **`-L`** stands for **list**, which displays the rules in all chains.
* **`-n`** stands for **numeric**, which prevents DNS lookups and keeps the output clean and fast.

***

### `iptables -I INPUT -p tcp --dport 5004 -j ACCEPT`
This command adds a **new rule** to the `iptables` firewall.
* **`-I INPUT`** inserts the rule at the beginning of the `INPUT` chain, which handles all incoming traffic.
* **`-p tcp`** specifies that the rule applies to the **TCP protocol**.
* **`--dport 5004`** specifies that the rule applies to traffic destined for port **5004**.
* **`-j ACCEPT`** is the **jump target** that instructs the firewall to **accept** (allow) any traffic that matches the rule.
