# Pull Docker Image

## Question

Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:

a. Pull busybox:musl image on App Server 2 in Stratos DC and re-tag (create new tag) this image as busybox:news.

## Answer

- Get the Secure Shell Access to App Server 01
```bash
sudo ssh tony@stapp01

sudo su -
```

- Pull the docker image with tag
```bash
docker pull busybox:musl

# Check if it downloaded correctly
docker images
```

- Create new tag
```bash
docket tag busybox:musl busybox:news

# Check again for new Tag appears
docker images
```

## How it works

docker pull [OPTIONS] NAME[:TAG|@DIGEST]

## Brief Description About the Environment

**What is happen in docker pull?**

Most of your images will be created on top of a base image from the Docker Hub registry.

Docker Hub contains many pre-built images that you can pull and try without needing to define and configure your own.

To download a particular image, or set of images (i.e., a repository), use docker pull.
