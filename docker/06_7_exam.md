# EXAM - 07

## Question

The Nautilus DevOps team is planning to do some cleanup on App Server 3 in Stratos Datacenter, some old and unused docker networks need to be deleted. Find below more details:

Delete a docker network named `php-network` from App Server 3 in Stratos Datacenter.

## Answer

```bash
# List down all the networks 
docker network ls

# remove network
docker network rm php-network
```