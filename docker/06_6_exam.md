# EXAM - 06

## Question

The Nautilus DevOps team is doing some cleanup work on all servers in Stratos DC. They were also looking for some unwanted and heavy docker images if present and can be cleaned.

On App Server 3 in Stratos Datacenter look for the docker images with size more than 100MB, delete all such docker images.

## Answer

```bash
# find what images size more than 100mb
docker images

# See whether any ontainer using that images.
docker ps

# if yes,
docker stop <container-name>
docker rm <container-name>

# now remove the images
docker rmi <image-name1> <image-name2>
```