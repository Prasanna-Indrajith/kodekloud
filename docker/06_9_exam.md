# EXAM - 09

## Question

There is a static website running within a container named nautilus, this container is running on App Server 3. Suddenly, we started facing some issues with the static website on App Server 3. Look into the issue to fix the same, you can find more details below:

a. Container's volume `/usr/local/apache2/htdocs` is mapped with the host volume `/var/www/html`.

b. The website should run on host port `8080` on App Server 3 i.e command `curl http://localhost:8080/` should work on App Server 3.

## Answer

```bash
# Compare Source and Destination with given directories
docker inspect nautilus | grep -i 'Source\|Destination'

# Create container
docker run -d -p 8080:80 -v /var/www/html:/usr/local/apache2/htdocs --name nautilus httpd:latest
```