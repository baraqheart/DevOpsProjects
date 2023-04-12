# Containerize a multi-tier project 

### 1. Launch an instance

spin a vm and connect through ssh in to the machine
install docker on the machine

### 2. containerize the application
 
- ***create a Dockerfile for database mysql***

`vim Dockerfile`

```
FROM mysql:5.7.25
LABEL "project"="vprofile-project"
LABEL "Author"="Baraqheart"

ENV MYSQL_ROOT_PASSWORD="vprodbpass"
ENV MYSQL_DATABASE

ADD db_backup.sql docker-entrypoint-initdb.d/db_backup.sql
```

- mysql image is our base image, version 5.7.25. 
- we also labeled our project passing the project name and the author.
- we set environmental variable for root pass to vprodbpass and database as 

- ***create a configuration file for nginx***

`vim nginvprofile.conf`


```
upstream vproapp {
 server vproapp:8080;
}
server {
  listen 80;
location / {
  proxy_pass http://vproapp;
}
}
```

- this is our nginx configuration file, that allows nginx runs or listen on port 80  and route requests to port 8080, 
- here, nginx servers as our load balancer for our application


- ***create a Dockerfile for web***

`vim Dockerfile`

```
FROM nginx
LABEL "project"="vprofile-project"
LABEL "Author"="Baraqheart"

RUN rm -rf /etc/nginx/conf.d/default.conf
COPY nginvprofile.conf /etc/nginx/conf.d/
```

- this dockerfile will host our web and the base image is ngingx, latest version as it wasnt specified.
- the run command removes any configuration file in the file path, and finally
- we will copy the nginvprofile.conf file we just created in to the directory /etc/nginx/conf.d/


- ***create a Dockerfile for app ***

`vim Dockerfile`

``
FROM tomcat:8-jre11
LABEL "project"="vprofile-project"
LABEL "Author"="Baraqheart"

RUN rm -rf /usr/local/tomcat/webapps/*
COPY target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war


EXPOSE 8080
CMD ["catalina.sh", "run"]
WORKDIR /usr/local/tomcat/
VOLUME /usr/local/tomcat/webapps
```

- this is our application dockerfile that hosts our complete code.
- tomcat is the base image and version is 8-jre11
- we have also labeled it to suit out need for description purpose
- on the RUN command, we will remove and current file or dir in our volume
- set our working directory to /usr/local/tomcat/
- set our volume also and export our port to 8080
- when we build our project, the artifact is stored in target/vprofile-v2.war, 
- but we want it on our volume, so the COPY command will copy it to our volume and will rename to ROOT.war
- cmd command runs our application


***note*** : we donot have our artifact yet, 
- this will be done manually and makes this project not fully automated
- to solve this problem we will try using a MULTI-STAGING technique in our next project! 
- remember, before we can build our images we need to create our artifact, by running the commands below

`sudo apt install open-jdk-8-jdk -y && sudo apt install maven -y`

- after installation completes, navigate to the target folder to see the artifact


- now lets update our backend service in application.properties file information
1. container name
2. username
3. password
4. database

copy the target directory that contains the artifact in to the app directory

` cp -r target Docker-files/app/ `

- now we will start by building our app images
 
` docker build -t <accountname>/<appimagename> . `

- build the image for web and database using the same code given above 
- confirm our images are built with ` docker images`
- now pull rabbitmq and memcached

- now have build and pull all our images to test and ensure its all working
- we will install compose if we dont have already, this will help run he image

- but to get our artifact, run the code bellow

` sudo apt install openjdk-8-jdk -y && sudo apt install maven -y ` 

- to build our artifact
` mvn install `

now check the target directory where we have our artifact

now copy 

cp -r target docker-files/app/

- now build our app image

### 4. writing a compose file
- open your prefered IDE, I use vscode
- and save it as docker-compose.yml, and paste the code below

```

version: '3'

Services: 
  vprodb:
      image: beeygold/vprofiledb:v1
      ports:
        - "3306:3306"
      volumes:
        - vprodbdata:/var/lib/mysql
      environment:
        - MYSQL_ROOT_PASSWORD=vprodbpass 
  vproweb:
      image: beeygold/vprofileweb:V1
      ports:
        - "80:80"
  vpromq01:
      image: rabbitmq
      ports:
        - "15672:15672"

  vprocache01:
      image: memcached
      ports:
        - "11211:11211"
      environment:
        - RABBITMQ_DEFAULT_USER=guest
        - RABBITMQ_DEFAULT_PASS=guest
    vproapp:
      image: beeygold/vprofileapp:V1
      ports:
        - "8080:8080"
      volumes:
        - vproappdata:/usr/local/tomcat/webapps

volumes:    
  vprodbdata: {}
  vproappdata: {}

```

- now to run our app, in the background
  
` docker-compose up -d `

go to the browser and we have this
















