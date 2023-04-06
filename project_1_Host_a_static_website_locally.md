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

now type `ls ` to see the new contents of the directory
` vi Vagrantfile `  to make changes to our file in order to configure the file
goto vagrant boxes [vagrant_box](https://app.vagrantup.com/boxes/search)  select a base image for cecntos7 linux operating system 
choose any one you preffer, we are going for "geerlingguy/centos7" now replace is with the base

- we gave our machine name vm01 
- remove the # symbol to uncomment the desired line
-such as private ip address and specified


bring up our machine with `vagrant up`

now you will see this 

***
[a1.png](https://github.com/baraqheart/HandsOn/blob/aba63d57355432119a1bfd8d0237e210aa4059ea/project_1/a1.PNG)

now let us login to our vm and install our website on the server

`vagrant ssh vm01`

although you can do this without specifiying the vm name as vm01
it is only necesary when we have created multiple virtual machine on the server

```
yum update && yum install httpd wget unzip
mkdir temp
cd temp
wget https://www.tooplate.com/zip-templates/2132_clean_work.zip
unzip 2132_clean_work.zip

cp -r 2132_clean_work/* /var/www/html/


***


