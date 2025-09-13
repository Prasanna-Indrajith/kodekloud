# EXAM - 08

## Question

The Nautilus DevOps team is planning to setup/create some docker containers on App Server 3 in Stratos Datacenter, some prerequisites are needs to be done on this server. Find below more details:

Create a new network named `mysql-network` using the `bridge` driver. Allocate subnet `182.18.0.0/24`, configure Gateway `182.18.0.1`.

## Answer

```bash
# Create network with all the given info
docker network create \
> --driver=bridge \
> --subnet=182.18.0.0/24 \
> --gateway=182.18.0.1 \
> mysql-network
```