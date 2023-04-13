# Launch Jenkins

### what is Jenkins

Jenkins is an open-source automation server that helps automate parts of the software development process. It is primarily used for continuous integration (CI) and continuous delivery (CD), which are practices that aim to improve the speed and quality of software development by automating the process of building, testing, and deploying code changes.

### 1. Create Security Group
First we will create a security group to allow ssh on port22 from any ip, 
allow http on port 80 and also custom port 8080  from any ip

### 2. Launch an instance
we will launch our instance to install jenkins on the server
select existing security group we created and insert the scipt below in the user data

```
#!/bin/bash

sudo apt update
sudo apt install open-jdk-8jdk -y
sudo apt install maven -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

```

when the machine is up, you can always confirm what script is in the user data by running 
`curl http://169.254.169.254/latest/user-data` this will fetch the user data on your machine
