# EXAM - 04

## Question

The Nautilus DevOps team has some confidential data present on App Server 3 in Stratos Datacenter. There is a container `ubuntu_latest` running on the same server. We received a request to copy some of the data from the docker host to the container. Below are more details about the task:

On App Server 3 in Stratos Datacenter copy an encrypted file `/tmp/nautilus.txt.gpg` from docker host to `ubuntu_latest` container (running on same server) in `/tmp/` location (create this location if doesn't exit). Please do not try to modify this file in any way.

## Answer

```bash
# copy file from local machine to container
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp
```