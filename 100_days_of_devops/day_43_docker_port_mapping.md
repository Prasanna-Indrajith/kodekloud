# Docker Port Mapping

## Question

The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 2 in Stratos Datacenter. Please perform the task as per details mentioned below:

a. Pull nginx:alpine docker image on Application Server 2.

b. Create a container named official using the image you pulled.

c. Map host port 8082 to container port 80. Please keep the container in running state.

## Answer

- SSH into App Server 02
```bash
ssh steve@stapp02
```

- Pull the `nginx:alpine` image
```bash
docker pull nginx:alpine
```

- Create docker container with port mapping using `nginx:alpine`
```bash
docker run -itd --name official -p 8082:80 nginx:alpine
```

## Brief Description About the Environment

**Port Mapping**

```bash
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                                            NAMES
b650456536c7        busybox:latest      top                 54 minutes ago      Up 54 minutes       0.0.0.0:1234->9876/tcp, 0.0.0.0:4321->7890/tcp   test

$ docker port test

7890/tcp -> 0.0.0.0:4321
9876/tcp -> 0.0.0.0:1234

$ docker port test 7890/tcp

0.0.0.0:4321

$ docker port test 7890/udp

2014/06/24 11:53:36 Error: No public port '7890/udp' published for test

$ docker port test 7890

0.0.0.0:4321
```

A network port is simply a way for network programs to identify which app they want to communicate with when making a connection request. For example, web servers typically listen on port 80 while MySQL database on 3306.

Now when running programs in containers, they end up with an isolated network stack. This means apps inside containers can only talk to other containers out of the box.

Port mapping creates a passage between the container and host to allow network requests to container apps from the outside world.

In other words, it maps a specified port on the Docker host IP to forward traffic to an application port inside the container.
Docker container port mapping diagram
Let‘s understand this better with a simple analogy.

Think of Docker containers like apartments in a fully contained housing complex. By default, these apartments can only communicate internally with other apartments in the complex.

Port mapping sets up a telephone line from selected apartment units to the leasing office outside. Now people can call those apartments directly through the leasing office transferring external calls to them!
