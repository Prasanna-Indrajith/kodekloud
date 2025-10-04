# Create Docker Network

## Question

The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:


a. Create a docker network named as news on App Server 1 in Stratos DC.


b. Configure it to use bridge drivers.


c. Set it to use subnet 10.10.1.0/24 and iprange 10.10.1.0/24.

## Answer

- ssh into App Server
```bash
ssh tony@stapp01
```

- Create the docker network using `bridge` driver and using `--subnet` and `--ip-range`
```bash
docker network create -d bridge 
--subnet 10.10.0.1/24 --ip-range 10.10.0.1/24 news
```

## Additional Details

**Network drivers**

`bridge`, `host`, `overlay`, `ipvlan`, `macvlan`, `none`

## Brief Description About the Environment

**Networking overview**

Container networking refers to the ability for containers to connect to and communicate with each other, or to non-Docker workloads.

Containers have networking enabled by default, and they can make outgoing connections. A container has no information about what kind of network it's attached to, or whether their peers are also Docker workloads or not. A container only sees a network interface with an IP address, a gateway, a routing table, DNS services, and other networking details. That is, unless the container uses the none network driver.
