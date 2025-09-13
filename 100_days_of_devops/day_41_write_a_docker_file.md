# Write a Docker File

## Question

As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file `/opt/docker/Dockerfile` (please keep D capital of Dockerfile) on App server 1 in Stratos DC and configure to build an image with the following requirements:

a. Use `ubuntu:24.04` as the base image.

b. Install `apache2` and configure it to work on `3001` port. (do not update any other Apache configuration settings like document root etc).

## Answer

- Go to desired directory and create `Dockerfile`
```bash
cd /opt/docker/

vi Dockerfile
```

- Write the process
```Dockerfile
# Use ubuntu:24.04 as the base image
FROM ubuntu:24.04

# Install Apache2 and its dependencies
RUN apt-get update && apt-get install -y apache2

# Copy a custom Apache configuration file to change the port
# The sed command directly modifies the default port in the Apache config
RUN sed -i 's/Listen 80/Listen 3001/' /etc/apache2/ports.conf

# Expose port 3001
EXPOSE 3001

# Run Apache in the foreground
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

- Create the docker image name `nautilus-apache` and tag `1.0`
```bash
docker build -t nautilus-apache:1.0 .
```

- Create new container and test using newly created image
```bash
bocker run -d -p 3001:3001 --name myContainer nautilus-apache:1.0

curl localhost:3001
```

## Brief Description About the Environment

**What is Dockerfile**

A **Dockerfile** is a text file that contains a series of commands and instructions used to automatically build a **Docker image**. Think of it as a blueprint or a recipe for creating a container. Instead of manually running commands to set up an environment, you list the steps in a Dockerfile, and the `docker build` command reads and executes them.

### **Core Concepts**

* **Layered Architecture**: Every instruction in a Dockerfile (like `RUN`, `COPY`, or `ADD`) creates a new, read-only layer in the Docker image. When you build the image, Docker caches these layers. If an instruction doesn't change, Docker uses the cached layer from a previous build, which makes the build process much faster.
* **Immutability**: Once an image is built, it's immutable. If you need to make a change, you simply edit the Dockerfile and rebuild the image, creating a new version. This makes it easy to track changes and roll back to previous versions.
* **Reproducibility**: Because the Dockerfile contains all the instructions, it ensures that anyone who builds the image from that file will get the exact same result. This is a key principle of a consistent and reliable development environment.

***

### **Common Instructions**

Here are some of the most common instructions found in a Dockerfile:

* **`FROM`**: Specifies the **base image** on which your image will be built. This is always the first instruction in a Dockerfile.
* **`RUN`**: Executes a command in a new layer on top of the current image. This is typically used for installing software, creating directories, or running system commands.
* **`COPY`**: Copies files or directories from the host machine to the filesystem of the image.
* **`CMD`**: Provides a default command to be executed when a container is launched from the image. Unlike `RUN`, this command is not executed during the build process.
* **`EXPOSE`**: Informs Docker that the container listens on the specified network ports at runtime. This is documentation and doesn't actually publish the port; the `-p` flag is required for that when running the container.
* **`WORKDIR`**: Sets the working directory for subsequent `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions.
* **`ENV`**: Sets environment variables in the container.