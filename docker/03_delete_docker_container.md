# Delete Docker Container

## Question

A container named kke-container was created by one of the Nautilus project developers on App Server 1. It was solely for testing purposes and now requires deletion. Execute the following task:

Delete the kke-container on App Server 1 in Stratos DC.

## Answer

- Log in to Application server 01
```bash
ssh tony@stapp01

sudo su
```

- `stop` the docker container
```bash
docker stop kke-container
```

- `remove` the docker container
```bash
docker rm kke-container
```
