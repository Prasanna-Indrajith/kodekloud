# Configure LAMP server

## Question

xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:

a. Install httpd, php and its dependencies on all app hosts.

b. Apache should serve on port 6000 within the apps.

c. Install/Configure MariaDB server on DB Server.

d. Create a database named kodekloud_db5 and create a database user named kodekloud_joy identified as password Rc5C9EyvbU. Further make sure this newly created user is able to perform all operation on the database you created.

e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud_joy

## Answer
### Using ansible

Create hosts_inventory file
```bash
vi hosts_inventory.ini
    [app_servers]
    stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_become_pass=Ir0nM@n
    stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_become_pass=Am3ric@
    stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33n

    [db_server]
    stdb01 ansible_host=172.16.239.10 ansible_user=peter ansible_ssh_pass=Sp!dy ansible_become_pass=Sp!dy
    
    :wq
```

Create playbook.yaml
```bash
vi playbook.yaml


```


```bash
# install ansible
sudo yum install ansible -y

ANSIBLE_HOST_KEY_CHECKING=False

```

### using bash terminal

Install `httpd,php` and enable port 6000 as serve
Need to run this code in all app servers.
```bash
ssh tony@stapp01

sudo su -

# install dependancies
yum install httpd php -y

systemctl enable httpd
systemctl start httpd 

vi /etc/httpd/conf/httpd.conf
 > Listen 80 > Listen 6000

systemctl reload httpd
```

On `database Server` 
```bash
yum install mariadb-server -y
sudo mysql_secure_installation

mariad
> CREATE DATABASE kodekloud_db2;
> CREATE USER 'kodekloud_aim'@'localhost' IDENTIFIED BY 'YchZHRcLkL';
> GRANT ALL PRIVILEGES ON kodekloud_db2.* TO 'kodekloud_aim'@'localhost'
> FLUSH PRIVILEGES;
```
## Additional Details

Additional commands with

Provide Description
```bash

```

## Brief Description About the Environment

**Apache**


