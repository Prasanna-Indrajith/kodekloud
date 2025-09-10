# Deploy Nginx Container on Application Server

## Question

The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 3. Complete the task with the following instructions:

On Application Server 3 create a container named nginx_3 using the nginx image with the alpine tag. Ensure container is in a running state.

## Answer

- Get Secure Shell access into Application Server 3
- Verify Docker is running
```bash
ssh banner@stapp03

systemctl status docker
```

- Pull the nginx image along with alpine tag
```bash
docker pull nginx:alpine
```

- Create and run the container named nginx_3
```bash
docker run -d --name nginx_3 nginx:alpine
```

- Verify container is running
```bash
docker ps --filter "NAME=nginx_3"
```

## Additional Details

`-d` runs the container in detached (background) mode so it stays running.
