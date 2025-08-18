# Setup SSL for Nginx

## Question

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 3 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:

1. Install and configure nginx on App Server 3.

2. On App Server 3 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

3. Create an index.html file with content Welcome! under Nginx document root.

4. For final testing try to access the App Server 3 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.

## Answer

```bash
# ssh into app server 03
ssh banner@stapp03

sudo su -

# install nginx
yum install nginx -y

# Enable and start nginx 
systemctl enable nginx
systemctl start nginx 

# Move certificate and key file into default ssl certificate and key directory (As I know from internet)
mkdir /etc/private /etc/certs

mv /tmp/nautilus.crt /etc/ssl/certs
mv /tmp/nautilus.key /etc/ssl/private/

# configure ssl connection by editing nginx `configuration` file
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup

vi /etc/nginx/nginx.conf

    server {
     listen 443 http2 ssl;
     listen [::]:443 http2 ssl;

     ssl_certificate /etc/ssl/nautilus.crt;
     ssl_certificate_key /etc/ssl/nautilus.key;
     ssl_dhparam /etc/ssl/dhparam.pem;

     root /usr/share/nginx/html;

     location / {
     }

     error_page 404 /404.html;
     location = /404.html {
     }

     error_page 500 502 503 504 /50x.html;
     location = /50x.html {
     }
    }

:wq

# Change delault nginx html directory's `index.html` file
echo "Welcome!" | tee /usr/share/nginx/html/index.html

# Reload nginx Server
nginx -t

systemctl reload nginx

exit
```

Now run curl command given in `jump_host`
```bash
curl -Ik https://172.16.238.12/
```

## Additional Details

**Command-line parameters**
nginx supports the following command-line parameters:

-? | -h — print help for command-line parameters.
-c file — use an alternative configuration file instead of a default file.
-e file — use an alternative error log file to store the log instead of a default file (1.19.5). The special value stderr selects the standard error file.
-g directives — set global configuration directives, for example, nginx -g "pid /var/run/nginx.pid; worker_processes `sysctl -n hw.ncpu`;"
-p prefix — set nginx path prefix, i.e. a directory that will keep server files (default value is /usr/local/nginx).
-q — suppress non-error messages during configuration testing.
-s signal — send a signal to the master process. The argument signal can be one of:
stop — shut down quickly
quit — shut down gracefully
reload — reload configuration, start the new worker process with a new configuration, gracefully shut down old worker processes.
reopen — reopen log files
-t — test the configuration file: nginx checks the configuration for correct syntax, and then tries to open files referred in the configuration.
-T — same as -t, but additionally dump configuration files to standard output (1.9.2).
-v — print nginx version.
-V — print nginx version, compiler version, and configure parameters.

## Brief Description About the Environment

**The document root in Nginx** is the directory from which the web server serves files. By default, it is typically set to `/usr/share/nginx/html` or `var/www/html`, but it can be changed in the Nginx configuration file.
