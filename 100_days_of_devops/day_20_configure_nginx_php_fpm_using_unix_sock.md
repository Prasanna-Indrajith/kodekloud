#  Configure Nginx + PHP-FPM Using Unix Sock

## Question

The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:

a. Install nginx on app server 1 , configure it to use port 8099 and its document root should be /var/www/html.
b. Install php-fpm version 8.1 on app server 1, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).
c. Configure php-fpm and nginx to work together.
d. Once configured correctly, you can test the website using curl http://stapp01:8099/index.php command from jump host.

## Answer

- SSh into App Server stapp01
- Install nginx 
```bash
ssh tony@stapp01

sudo su -

yum install nginx -y
```

- Enable php 8.1
- Install php and php-fpm
```bash
sudo dnf module reset php -y
sudo dnf module enable php:8.1 -y

sudo dnf install -y php php-fpm php-mysqlnd
```

- Create socket director (It's not exist)
```bash
sudo mkdir -p /var/run/php-fpm
```

- Configure the php-fpm
```bash
vi /etc/php-fpm.d/www.conf
```

```vi
listen = /var/run/php-fpm/default.sock
listen.mode = 0660
user = nginx
group = nginx
```

- Create seperate confiuration file for new application with enabling port 8099
- Configure Nginx `/etc/nginx/conf.d/newapp.conf`
```bash
sudo vi /etc/nginx/conf.d/newapp.conf
```

- Inside `newapp.conf`
```newapp.conf
server {
    listen 8099;
    root /var/www/html;
    index index.php index.html;

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

All Done. Now we can `enable` and `start` the services.
```bash
systemctl enable nginx
systemctl start nginx 

systemctl enable php-fpm
systemctl start php-fpm
```

Testing the application cofiguration.
```bash
curl http://stapp02:8099/index.php
```

```OUTPUT
Welcome to xFusionCorp Industries!
```
## Additional Details

The two commands `sudo dnf module reset php -y` and `sudo dnf module enable php:8.1 -y` are used together to manage different versions of PHP on a Red Hat-based Linux system. This method is part of dnf's module system, which allows you to install multiple versions of a software stack side-by-side.

## Brief Description About the Environment

**What is php-fpm?**

PHP-FPM stands for **PHP FastCGI Process Manager**. It's an alternative to the standard PHP FastCGI implementation that comes with additional features useful for high-traffic websites. Instead of having a web server like Apache or Nginx handle PHP requests directly, PHP-FPM manages a pool of PHP processes that are separate from the web server.

### How it Works

1.  A user's web browser sends a request for a PHP file (like `index.php`).
2.  The web server (e.g., Nginx or Apache) receives this request.
3.  The web server recognizes it as a PHP file and passes the request to PHP-FPM.
4.  PHP-FPM processes the request using one of its available worker processes. 
5.  PHP-FPM sends the output back to the web server.
6.  The web server then sends the final HTML output back to the user's browser.

### Key Features and Advantages

* **Performance:** PHP-FPM's process pooling allows it to handle multiple concurrent requests efficiently. It can be configured to manage a specific number of processes, which helps control server resource usage.
* **Stability:** If a single PHP-FPM worker process crashes, it doesn't bring down the entire web server. The process is simply replaced, which improves the overall stability of the application.
* **User Management:** PHP-FPM allows you to run different PHP websites with different user IDs. This is a significant security benefit, as it isolates sites from each other.
* **Advanced Configuration:** It provides more granular control over PHP settings and logging compared to a traditional PHP setup. This includes features like slow-log logging, which helps in debugging performance issues.
