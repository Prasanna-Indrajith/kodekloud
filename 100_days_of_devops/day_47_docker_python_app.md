# Docker python App

## Question

A python app needed to be Dockerized, and then it needs to be deployed on App Server 3. We have already copied a requirements.txt file (having the app dependencies) under `/python_app/src/` directory on `App Server 3`. Further complete this task as per details mentioned below:

Create a Dockerfile under `/python_app` directory:

Use any `python` image as the base image.
Install the dependencies using `requirements.txt` file.
Expose the port `6000`.
Run the `server.py` script using `CMD`.

Build an image named `nautilus/python-app` using this Dockerfile.

Once image is built, create a container named `pythonapp_nautilus`:

Map port `6000` of the **container** to the **host port** `8096`.

Once deployed, you can test the app using curl command on App Server 3.

`curl http://localhost:8096/`

## Answer

- SSH into App server 03 and go into corresponding directory
- And create Dockerfile
```bash
ssh banner@stapp03

sudo su

cd /python_app

vi Dockerfile
```

- In that `Dockerfile
- write the process
```docker
FROM python:3

WORKDIR /usr/src/app

COPY ./src/requirements.txt ./
RUN pip install requirements.txt

COPY ./src/server.py ./

CMD ["python", "server.py"]
```

- Time to test
- `build` and image using out `Dockerfile`
```bash
docker build -t nautilus/python-app .
```

- Now create and run a container using previously created image
```bash
docker run -d --name pythonapp_nautilus -p 8096:6000 nautilus/python-app
```

## Additional Details

Additional commands with

Provide Description
```bash
# view ongoing processes
docker ps 

# View all the prevoiusly runned processes
docker ps -a
```