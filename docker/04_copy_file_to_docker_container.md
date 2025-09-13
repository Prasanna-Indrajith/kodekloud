# Copy File to Docker Container

## Question

The Nautilus DevOps team possesses confidential data on App Server 3 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.

Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /home/. Ensure the file is not modified during this operation.

## Answer

- list app the processes currently running
```bash
docker ps -a
```

- Copy file using `docker cp` 
```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home
```

## Additional Details

- Ensure file is copied correctly
```bash
docker exec -it ubuntu_latest "/bin/bash"
```

