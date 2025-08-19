# Install and configure nginx as an LBR

## Question

Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:

a. Install nginx on LBR (load balancer) server.

b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.

d. Once done, you can access the website using StaticApp button on the top bar.

## Answer

SSH into `App Server 01` for which **port** apache is gonna serve the web application to Load Balancer.
```bash
ssh tony@stapp01

sudo su -

systemctl status httpd
# In my case apache isn't installed on app server 01. So I did install it.

yum install httpd -y

systemctl enable httpd

systemctl start httpd

# The port app server 01 is gonna serve the webpage is can be find in
cat /etc/httpd/conf/httpd.conf | grep Listen
    > Listen 8082 < # in my case
```

SSH into `Load Balancer` Sever and do the stuffs on it.
```bash
ssh loki@stlb01

sudo su -

# Install nginx 
yum install nginx -y

systemctl enable nginx

systemctl start nginx

# configure the LOAD_BALANCER
vi /etc/nginx/nginx.conf
```

Add these info as your port and hostnames inside the `http` block
  ```nginx.conf

    http {
        upstream app_servers {
            server stapp01.stratos.xfusioncorp.com:8082;
            server stapp02.stratos.xfusioncorp.com:8082;
            server stapp03.stratos.xfusioncorp.com:8082;
        }

        server {
            listen 80;

            location / {
                proxy_pass http://app_servers;
            }
        }
    }
```

test nginx and reload nginx
```bash
nginx -t

systemctl reload nginx
```

## Brief Description About the Environment

Nginx can be configured as a load balancer to distribute traffic across multiple servers, improving performance and reliability. It supports various load balancing methods, including round-robin and least-connected, allowing for efficient resource utilization.

**Load balancing methods**
The following load balancing mechanisms (or methods) are supported in nginx:

1. round-robin — requests to the application servers are distributed in a round-robin fashion,
2. least-connected — next request is assigned to the server with the least number of active connections,
3. ip-hash — a hash-function is used to determine what server should be selected for the next request (based on the client’s IP address).
