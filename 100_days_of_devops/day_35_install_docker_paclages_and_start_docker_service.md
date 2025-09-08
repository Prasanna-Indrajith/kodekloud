# Install Docker Packages and Start Docker Service

## Question

The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:

Install `docker-ce` and docker compose packages on App Server 2.

Initiate the docker service.

## Answer

- Get super user Secure Shell Access to App Server 02
```bash
ssh steve@stapp02

sudo su -
```

- Setup the repository (Install the dnf-plugins-core package (which provides the commands to manage your DNF repositories) and set up the repository.)
```bash 
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

- Install required docker packages
```bash
 sudo dnf install docker-ce docker-compose-plugin
```

- Start the service
```bash
sudo systemctl enable --now docker
```

## Brief Description About the Environment

**Docker**

Docker Engine is an open source containerization technology for building and containerizing your applications. Docker Engine acts as a client-server application with:

- A server with a long-running daemon process dockerd.
- APIs which specify interfaces that programs can use to talk to and instruct the Docker daemon.
- A command line interface (CLI) client docker.

Installation Process [Read More](https://docs.docker.com/engine/install/centos/)
