# Host a static website locally
 Vagrant is an open-source tool used for building and managing virtual development environments. It enables developers to easily set up and configure virtual machines, - providing a consistent development environment across multiple platforms.

- install vagrant on your local machine 
- navigate to the vagrant website [vagrant](https://developer.hashicorp.com/vagrant/downloads) and choose your operating system.
- for windows user download the msi file and install
- once it installed, open git bash to confirm, type ` vagrant -v `

you shoud see the varant version information

now lets launch a virtual machine with centos7 operatine system

```
cd Documents
mkdir local_deployment
cd local_deployment
vagrant init
```

now you should recieved a response that its successful
***
![a1.png](https://github.com/baraqheart/HandsOn/blob/aba63d57355432119a1bfd8d0237e210aa4059ea/project_1/a1.PNG)

***

### Configure the Virtual machine

now type `ls ` to see the new contents of the directory
` vi Vagrantfile `  to make changes to our file in order to configure the file
goto vagrant boxes [vagrant_box](https://app.vagrantup.com/boxes/search)  select a base image for cecntos7 linux operating system 
choose any one you preffer, we are going for "geerlingguy/centos7" now replace is with the base
- remember the private ip we assigned to our vm http://192.168.33.50/, this will be the ip through which we will access our website

- we gave our machine name vm01 
- remove the # symbol to uncomment the desired line
-such as private ip address and specified
***
![editpage](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_1/a2.PNG)

bring up our machine with `vagrant up`

now you will see this 

![vm](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_1/a6.PNG)
***

now let us login to our vm and install our website on the server

`vagrant ssh vm01`

although you can do this without specifiying the vm name as vm01
it is only necesary when we have created multiple virtual machine on the server


```
sudo -i
yum update && yum install httpd wget unzip -y

systemctl start httpd
systemctl enable httpd

mkdir temp
cd temp

wget https://www.tooplate.com/zip-templates/2129_crispy_kitchen.zip
unzip 2129_crispy_kitchen.zip

cp -r 2129_crispy_kitchen/* /var/www/html/
systemctl restart httpd

```

***
![homepage](https://github.com/baraqheart/HandsOn/blob/8f3c2035b029c9f0feb9bd63634f0f1df83cc5bf/project_1/a7.PNG)


