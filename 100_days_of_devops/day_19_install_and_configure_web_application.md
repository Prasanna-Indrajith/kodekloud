# Install and configure web application

## Question

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:

a. Install httpd package and dependencies on app server 1.

b. Apache should serve on port 8086.

c. There are two website's backups /home/thor/blog and /home/thor/cluster on jump_host. Set them up on Apache in a way that blog should work on the link http://localhost:8086/blog/ and cluster should work on link http://localhost:8086/cluster/ on the mentioned app server.

d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:8086/blog/ and curl http://localhost:8086/cluster/

## Answer

Copy 2 websites(blog, cluster) into App Server 01 /home/tony/
```bash
sudo scp -r /home/thor/blog /home/thor/cluster tony@stapp01:/home/tony

#ssh into Tony
ssh tony@stapp01

sudo su -
```

Install the apache(httpd) service and enable and setup it.
```bash
yum install httpd -y

systemctl enable httpd

systemctl start httpd
```

Set the Listing port as 8086. Move copied two websites into `/var/www/html` directory.
```bash
sed -i "s/80/8086/g" /etc/httpd/conf/httpd.conf

mv /home/tony/apps /home/tony/cluster /var/www/html

# if there is not directory called www or html
# create it there.
```

Restart `httpd` and testing results.
```bash
systemctl restart httpd

curl http://localhost:8086/blog/
curl http://localhost:8086/service/
```

## Additional Details

`sed` - for replace file's content
