
# AUTOMATE JENKINS LAUNCH USING TERRAFORM

### 1. Install Terraform 

- spin an ubuntu vm and connect through ssh in to the machine
- to install terraform, visit the site [Terraform_install](https://developer.hashicorp.com/terraform/downloads)  to ensure 
  you always have the updated
- version of the instalation code
- choose the os, you want to install and follow along  


```
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

```

### 2. verify Terraform is installed

- to verify terraform has been installed, try this
`terraform -v`

### 3. Jenkins Installation script

- Before we start writing our terraform files we will write jenkins installation script goto the link [jenkins_install] 
  (https://www.jenkins.io/doc/book/installing/linux/) to get updated version always
- note: we are on an ubuntu machine, so we will also  have jenkins ubuntu installation process.
- lets create a file to save our script `touch jenkins.sh` and copy and paste the code below.


```
#!/bin/bash
sudo apt update
sudo apt install maven -y
sudo apt install openjdk-11-jdk -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
```

- The first part of the code after the shabang is updating and downloading dependencies for jenkins to function properly
- and the last part is download and install jenkins on the server
