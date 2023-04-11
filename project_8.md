# Containerize a flask application
in this project, we will containerize a flask project in few steps

goto [dockerdemo](https://docs.docker.com/compose/gettingstarted/) to see the full details on this demo project


### STEP 0: install docker
### launch an instance (ec2 or vm)
- after launching instance, log in to the acount
- ensure u allow all port from <my_ip> on security group since we will be using different port

# STAGE 2
### install dependencies and docker on the machine

- install as a root user

- note: always install through documentation page, updates might have been made on the installation to avoid error

- installation steps for docker engine on ubuntu

### a. Set up the repository
- Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
```

- Add Docker’s official GPG key:

```
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- Use the following command to set up the repository:

```
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```


```
sudo apt-get update 
```

- Install Docker Engine, containerd, and Docker Compose.

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- Verify that the Docker Engine installation is successful by running the hello-world image:

```
sudo docker run hello-world
```

***


### STEP 1: create the app

```
mkdir composetest
cd composetest
touch app.py
```

add the following code below to the app.py file
```
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

***
![images](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2011-24-04.png)

![image2](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2011-28-50.png)

- Create another file called requirements.txt in your project directory and paste the following code in:

```
flask
redis
```

### STEP 2: create the Docker file 
we will create a Dockerfile by ` vim Dockerfile` and paste the code below

```
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

***
![img2](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2011-30-49.png)


- this file interpretes to :
- Build an image starting with the Python 3.7 image.
- Set the working directory to /code.
- Set environment variables used by the flask command.
- Install gcc and other dependencies
- Copy requirements.txt and install the Python dependencies.
- Add metadata to the image to describe that the container is listening on port 5000
- Copy the current directory . in the project to the workdir . in the image.
- Set the default command for the container to flask run.

### STEP 3: create docker-compose file

create a docker-compose file by ` vim docker-compose` to define our service

```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

***

![img2](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2011-41-56.png)



The web service uses an image that’s built from the Dockerfile in the current directory.
It then binds the container and the host machine to the exposed port, 8000. 
This example service uses the default port for the Flask web server, 5000.

The redis service uses a public Redis image pulled from the Docker Hub registry.

###. Building our image and running our container

```
docker build -t .
```
***
![img](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2012-46-23.png)


```
 docker compose up
```
![](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2013-06-48.png)

### validate
to check and see that our app has been containerized and now working properly
go to the public ip of your ec2 or vm to check and now we can see its a success!

***
![](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2012-45-52.png)

refresh the page 3times and make little changes to the app.py file you will see how dynamic it is

![img3](https://github.com/baraqheart/HandsOn/blob/fdd0df8fe0e3fb2e34eaf06cf863c7f1ef7d095c/python/Screenshot%20from%202023-03-13%2013-05-49.png)


