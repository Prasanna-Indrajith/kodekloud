# Troubleshoot Docker Container Issue

## Question

An issue has arisen with a static website running in a container named nautilus on App Server 1. To resolve the issue, investigate the following details:

Check if the container's volume /usr/local/apache2/htdocs is correctly mapped with the host's volume /var/www/html.

Verify that the website is accessible on host port 8080 on App Server 1. Confirm that the command curl http://localhost:8080/ works on App Server 1.

## Answer

- Check whether site is accessible
```bash
curl localhost:8080
```

- Check for container `Source` and `Destination`
```bash
docker inspect nautilus | grep -i 'Source\|Destination'

# OK <- in my case
```

- Check docker current processes
```bash
docker ps -a 

# exited - in my case
```

- So I have to start it and check whether it's working with `curl`
```bash
docker start nautilus

# nautilus <- is the name of my container

curl localhost:8080
```

- It's working!

## Brief Description About the Environment

**Volumes in Docker**

Docker volumes are a persistent storage mechanism that decouples your data from the container lifecycle. Volumes let containers retain data across restarts and removals, and allow multiple containers to share a common data source. Unlike bind mounts, Docker manages volumes internally and stores them under /var/lib/docker/volumes/ on Linux systems.
