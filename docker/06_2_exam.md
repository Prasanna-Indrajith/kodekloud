# EXAM - 02

## Question

The Nautilus team wants to create a debug container on Application Server 3. However, they had some specific requirements related to the CMD. Please complete the task as per details given below:

a. On Application Server 3 create a container named `debug_3` using image `ubuntu/apache2:latest`.

b. Overwrite the default CMD with command `sleep 1000`.

c. Make sure the container is in running state.

## Answer

```bash
# List down current docker images
docker images

# Pull image
docker pull ubuntu/apache2:latest
docker images

# Create container
docker run -d --name debug_3 ubuntu/apache2:latest sleep 1000
docker start debug_3

# Check it's in running state or not
nginx ps -a 
```