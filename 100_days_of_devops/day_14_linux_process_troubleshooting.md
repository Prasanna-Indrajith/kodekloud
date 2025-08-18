# Linux process troubleshooting

## Question

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.

Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 6300 on all app servers.

## Answer

Send https request to each app server and where is the faulty server located.
```bash
curl stapp01:6300
curl stapp02:6300
curl stapp03:6300

# mine faulty server is `app server 01`
```

Enable and start the server to see is there any issue there.
```bash
sudo su -

systemctl enable httpd

systemctl start httpd # Here is the fauly start 

# See where is the fault 
systemctl status httpd # basically 6300 port is using some else program

# See what program use 6300 port
netstat -tupln | grep 6300 # < in my case exmaple program id is 1234

# Kill the process
kill 1234
```

Start again httpd and see what happen
```bash
sytemctl start httpd
systemctl status httpd
exit
```

send http request using `curl` from `jump_host`
```bash
curl stapp01:6300
```

