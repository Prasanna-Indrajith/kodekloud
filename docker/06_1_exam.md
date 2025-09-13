# EXAM - 01

## Question

The Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 3. Please complete the task as per details given below:

On Application Server 3 create a container named `nginx_3` using image `nginx` with `alpine` tag and make sure container is in running state.

## Answer

```bash
#ssh into application Server 03
ssh banner@stapp03

# List down current docker images
docker images

# Pull nginx image with alpine tag
docker pull nginx:apline
docker images

# Create nginx_3 container & start it
docker run -d --name nginx_3 nginx:apline
docker start nginx_3

# Check it's in running state or not
nginx ps -a 
```