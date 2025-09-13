# EXAM - 05

## Question

We need some docker images on Application Server 3, these images will be used to create some containers later. Pull below mentioned images on Application Server 3 in Stratos DC.

a. redis:alpine
b. memcached:alpine

## Answer

```bash
# pull dockert images
docker pull redis:alpine
docker pull memcached:alpine

# List down images
docker images
```