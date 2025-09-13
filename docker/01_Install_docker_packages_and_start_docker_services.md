# Install Docker Packages and Start Docker Service

## Question

The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:

Install docker-ce and docker compose packages on App Server 2.

Initiate the docker service.

## Answer

- Install the `yum-utils` package, which provides the yum-config-manager utility, then add the official Docker repository to your system.
```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

- Install Docker CE and Docker Compose
```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- Enable docker and check the installation
```bash
systemctl enable --now docker

docker --version
docker compose version
```

## Brief Description About the Environment

**Docker CE**

Docker CE (Community Edition): This is the free, open-source version of the Docker platform. It's intended for individual developers and small teams. When you install Docker from the official repository on a Linux server (using a command like yum install docker-ce), you're installing Docker CE. It includes the Docker Engine, CLI, and all the core components needed to build and run containers.

**Docker Compose** 

When people refer to "Docker," they're often talking about the entire platform, but from an installation and licensing perspective, it's more precise to use the terms Docker CE, Docker EE, or Docker Desktop. Docker Compose is a separate tool that works with Docker to manage multi-container applications.

Docker CE vs. "Docker"
There is no single package or product named just "Docker" anymore. Instead, the term has become a generic name for the platform.

Docker CE (Community Edition): This is the free, open-source version of the Docker platform. It's intended for individual developers and small teams. When you install Docker from the official repository on a Linux server (using a command like yum install docker-ce), you're installing Docker CE. It includes the Docker Engine, CLI, and all the core components needed to build and run containers.

Docker EE (Enterprise Edition): This is the commercial version of Docker, now managed by Mirantis. It's designed for large enterprises and includes additional features like enhanced security, official support, and centralized management tools.

Docker Desktop: This is a separate, easy-to-install application for Windows, macOS, and Linux that bundles Docker Engine (CE) with a user-friendly graphical interface, a single-node Kubernetes cluster, and other developer tools. It's not for production servers.

In short, when you say "install Docker," you're almost always referring to installing Docker CE or Docker Desktop.

What is Docker Compose?
Docker Compose is a tool that simplifies the management of multi-container Docker applications. While the main docker command is great for managing a single container, most real-world applications consist of multiple services (e.g., a web server, a database, and a cache).

Docker Compose allows you to define all your application's services in a single YAML file named `docker-compose.yml`. This file specifies:

- Which Docker images to use for each service.

- How to link the containers together.

- Which ports to expose.

- Any volumes or environment variables.

With this single file, you can bring up, scale, or tear down your entire application stack with a single command like docker compose up. This saves you from having to run a series of complex docker run commands for each container.
