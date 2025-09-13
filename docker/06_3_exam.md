# EXAM - 03

## Question

We received a request to copy some of the data from one of the docker containers to the docker host. The container is running on App Server 3 in Stratos Datacenter. Below are more details about the task:

On App Server 3 in Stratos Datacenter copy an encrypted file `/tmp/test.txt.gpg` from `development_3` docker container to the docker host in `/tmp` location. Please do not try to modify this file in any way.

## Answer

```bash
# copy file from container to current machine
docker cp development_3:/tmp/test.txt.gpg /tmp
```