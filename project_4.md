# Launch an EC2 instance

- enter the name of your instance
- choose an AMI

![type](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a6.PNG)


- create a key pair
- select instance type 

***
![key](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a3.PNG)





- configure security group to allow connection on port 22 and 80 from anywhere- 

***
![sg](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a4.PNG)
***


***
****


****

- paste in the user data to install neccesary package that powers our website

```
#!/bin/bash

# Elevate privileges to root
sudo -i

# Install required packages
yum update && yum install httpd wget unzip -y

# Start the Apache service and enable it to start automatically on boot
systemctl start httpd
systemctl enable httpd

# Download and extract the website template to a temporary directory
mkdir temp
cd temp
wget https://www.tooplate.com/zip-templates/2129_crispy_kitchen.zip
unzip 2129_crispy_kitchen.zip

# Copy the extracted files to the Apache document root directory
cp 2129_crispy_kitchen/* /var/www/html/

# Restart the Apache service to load the new website
systemctl restart httpd

```

***

- try connect to the instance with the public ip address and the key pair 

![login](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a7.PNG)

- note the ip address keeps changing as it is not static

(![home]https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a9.PNG)


## Load Balancer with EC2
- we will 2 different website but will access them through load balancer
- application load balancer with route requests to either of the website as we refresh the page it will be dynamic
- switch from one site to another through one address

1. to get started, we will create a target group and select both instances

![tg1](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a00.PNG)

2. the target group will perform health checks on the instances before allowing load balancer to route requests to it


![tg](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/a12.PNG)

3. we will configure our load balance to route request to both instances

4. we will configure our instance to allow load balancer to route requests to it

![lb](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/13.PNG)

5.we will validate if it works or not

![home](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/lb3.PNG)

![home2](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_4/lit3.PNG)

it works!!


- 

