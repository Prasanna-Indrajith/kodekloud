# Deploy an App on Docker Container

## Question

The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:



On App Server 3 in Stratos Datacenter create a docker compose file /opt/finance/docker-compose.yml (should be named exactly).


The compose should deploy two services (web and DB), and each service should deploy a container as per details below:


For web service:


a. Container name must be php_web.


b. Use image php with any apache tag. Check here for more details.


c. Map php_web container's port 80 with host port 6400


d. Map php_web container's /var/www/html volume with host volume /var/www/html.


For DB service:


a. Container name must be mysql_web.


b. Use image mariadb with any tag (preferably latest). Check here for more details.


c. Map mysql_web container's port 3306 with host port 3306


d. Map mysql_web container's /var/lib/mysql volume with host volume /var/lib/mysql.


e. Set MYSQL_DATABASE=database_web and use any custom user ( except root ) with some complex password for DB connections.


After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:6400/

For more details check here.


Note: Once you click on FINISH button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

## Answer

- SSH into Application Server 03 and go into directory they mentioned.
```bash
ssh banner@stapp03

sudo su

cd /opt/finance

vi docker-compose.yml
```

- In `docker-compose.yml` file
```bash
version: '2.29.1' # Specifies the Docker Compose version

services:
  # Web service definition
  php_web:
    container_name: php_web # Sets the container name
    image: php:8.3-apache # Uses a PHP image with Apache (you can choose any Apache tag)
    ports:
      - "6400:80" # Maps host port 6400 to container port 80
    volumes:
      - /var/www/html:/var/www/html # Mounts a host directory to the container
    depends_on:
      - mysql_web # Ensures the database container starts before the web container

  # Database service definition
  mysql_web:
    container_name: mysql_web # Sets the container name
    image: mariadb:latest # Uses the latest MariaDB image
    ports:
      - "3306:3306" # Maps host port 3306 to container port 3306
    volumes:
      - /var/lib/mysql:/var/lib/mysql # Mounts a host directory for persistent data
    environment:
      - MYSQL_DATABASE=nautilusdb# Sets the database name
      - MYSQL_USER=nautilusdbboss # Creates a custom user
      - MYSQL_PASSWORD=nautilusdbboss # Sets a password for the custom user
      - MYSQL_ROOT_PASSWORD=nautilusdbroot # Sets the root password (required for setup)
```

- Test run of `docker-compose.yml` file
```bash
docker compose up -d 
```

## Additional Details

- In `Dockerfile` we only can `build` one container at a time.
- But with `docker compose` we can create multiple containers with single `yml` file 

## Brief Description About the Environment

You create a single `.yml` file for this task using **Docker Compose**. The power of Docker Compose is that it allows you to define multiple services (containers) and their configurations in one central file. The `services` key in the file is where you define each container.

### How it's possible

The `docker-compose.yml` file is structured to define an entire application stack. Each item listed under the `services` key represents a separate container that Docker Compose will create and run. This file essentially acts as a single manifest for your multi-container application, handling networking and inter-container communication for you.
