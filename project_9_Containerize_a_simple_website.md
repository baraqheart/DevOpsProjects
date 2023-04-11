# Containerize a simple website 
### Tools
- docker
- ec2 or vm
- dockerhub account

# STAGE 1
### launch an instance (ec2 or vm)
- after launching instance, log in to the acount
- ensure u allow all port from <my_ip> on security group since we will be using different port

# STAGE 2
### install dependencies and docker on the machine

- install as a root user

- note: always install through documentation page, updates might have been made on the installation to avoid error

- installation steps for docker engine on ubuntu

# a. Set up the repository
- Update the apt package index and install packages to allow apt to use a repository over HTTPS:


```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
 ```
 

# b. Add Docker’s official GPG key:


```
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

# c.Use the following command to set up the repository: 

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

# update again

```
sudo apt-get update 
```

# Install Docker Engine, containerd, and Docker Compose

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# after installation complete check to comfirm docker is active

```
systemctl status docker
```

```
sudo service docker start
sudo docker run hello-world
```

- logout from root user to (ubuntu, azureuser, ec2-user)

- note to add your user to the group that gives permission

```
sudo vim /etc/group
```
or

```
sudo usermod -aG docker <username>
```

- note once you are done with this logout confirm and login and logout again and try

```
docker images #without adding sudo to the command 
```

# STAGE 3. prepare an artifact
- create a working directory

```
mkdir images/crispy_kitchen
```

# dowload a zip file for html templates from tooplate.com

```
wget https://www.tooplate.com/zip-templates/2129_crispy_kitchen.zip
```

# install unzip if not available

```
sudo apt install unzip
```

# unzip files

```
unzip 2129_crispy_kitchen.zip
```

- check that the zipped file has been unzipped
- and delete unwanted files and navigate into the unzipped filder

```
cd 2129_crispy_kitchen 
```

- create the archive with the name you intend to name the artifact and what to include in the archive

```
tar czvf crispy.tar.gz *
```

- create a Dockerfile with no extension in the same directory

```
vim Dockerfile
```

- paste this below

```
FROM ubuntu:latest

LABEL "Author "="<yourname>"
LABEL "Project"="crispy_kitchen"

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && apt install git -y
RUN apt install apache2 -y

CMD ["/usr/sbin/apache2ctl","-D", "FOREGROUND"]

EXPOSE 80

WORKDIR /var/www/html
VOLUME /var/log/apache2

ADD crispy.tar.gz /var/www/html
#COPY crispy.tar.gz /var/www/html
```

- open your web browser and navigate to hub.docker.com site to create your docker account where your repository will be stored

- login to docker from terminal
- docker login

- enter your username and password

- now build our image

```
docker  build -t <account_name>/crispy_kitchen .
```

- run your image with

`docker run -d -n crispysite -p 9080:80 <account_name>/crispy_kitchen`

***
![]()

***
![]()

***
![]()

***
![]()

***
![]()
