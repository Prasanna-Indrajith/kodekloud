# Deploy Nginx Container on Application Server

## Question

The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 1. Complete the task with the following instructions:

On Application Server 1 create a container named nginx_1 using the nginx image with the alpine tag. Ensure container is in a running state.

## Answer

- Log in to Application server 01
```bash
ssh tony@stapp01

sudo su
```

- Pull the docker nginx image 
```bash
docker pull nginx:alpine

# View the downloaded image
docker images
```

- Start the nginx container
```bash
docker run --name nginx_1 -d nginx:alpine
```

- View the running Details
```bash
docker ps -a
```

## Brief Description About the Environment

**What is Docker NGINX?**

Docker NGINX refers to running the NGINX web server inside a lightweight container. Docker containers are similar to small packages that hold an application and all the dependencies it needs. 

A typical Docker image for NGINX includes the server software, a default configuration, and a directory structure for hosted files. You can run that container right away or adjust it by mapping specific folders, adjusting configuration files, or building an image based on your own Dockerfile. By doing that, you might add custom modules or scripts to customize NGINX for your application.


