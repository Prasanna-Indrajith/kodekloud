# Docker EXEC Operations

## Question

One of the Nautilus DevOps team members was working to configure services on a `kkloud` container that is running on App Server 1 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:

a. Install `apache2` in `kkloud` container using `apt` that is running on App Server 1 in Stratos Datacenter.

b. Configure Apache to listen on port `5003` instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, `127.0.0.1`, container ip, etc.

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

## Answer

- SSH into App server 3
- Log in to the container to install Apache
```bash
docker exec -it kkloud "/bin/bash"
apt install apache2
```

- Edit `/etc/apache2/ports.conf` - `80` -> `5003` (default por)
- Edit `000-default.conf` -> `HostName=localhost` (virtual host configuration)

- Restart Apache and check the status again
```bash
service apache2 restart
```

- Check the connection
```bash
curl localhost:5003
```