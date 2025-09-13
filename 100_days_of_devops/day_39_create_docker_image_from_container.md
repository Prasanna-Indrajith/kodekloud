# Create a Docker Image From Container

## Question

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:

a. Create an image games:nautilus on Application Server 1 from a container ubuntu_latest that is running on same server.

## Answer

- Get the Secure Shell Access to App Server 01
```bash
sudo ssh tony@stapp01
```

- Look for already running images
```bash
docker images
```

- Create the image from `ubuntu_latest` container
```bash
docker commit -m "snapshot from ubuntu_latest" -a "dev" ubuntu_latest games:nautilus
```

