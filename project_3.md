# Deploy a multi-tier web application Locally

in this project we will build a multi tier application locally 
we will divide it to two parts, in the first part, we will build it manually
and in the second part, we will automate our deployment

pre requisite tools
- Virtual box
- Vagrant
- Git bash
- IDE e.g vscode, notepad++
- vagrant hostmanager plugin
 

we will be setting up in this order

- mysql for our database
- memcache for caching
- rabbitmq for our message broker
- tomcat for our application
- nginx for web server

# part 1: Manually Deploy a multi-tier application Locally

- firstly, we will install hostmanager plugin `vagrant plugin install    vagrant-hostmanager` this is used to manager multiple machines in one file
- next, we will initailize vagrant with `vagrant up`
- now we will edit the Vagrantfile, and clean up the comments to make it easy to read and work with
- we will duplicate all the code 5 times and edit it, feel free to use any ide of your choice at this point as we will be powering 5 machines all ounce in a file
- carefully edit only the neccesary parts for our project

below are the names for our virtual machines

- db01: this will store and manage our database
- mc01: memcached will cache our data 
- rmq01: message broker server
- web01: the web server will serve as our load balancer to the app01 server
- app01: the app server hosts our main application 

we will start by powering up our machine after editing and configuring our Vagrantfile to this

```

```

majorly, what we are doing here is to configure each of the machines, specifying its hostname, operating system, RAM size and private ip address

once everything is set  `vagrant up` to bring up all our machines
now all our machines are up, lets validate

`vagrant ssh <hostname>` and you will see information about the host

try `vagrant ssh web01` and check `cat /etc/host ` 
`ping app01` and see sa successful ping

### 1. setting up the database
`vagrant ssh db01`
`vi /etc/profile`

add this to the last line 
DATABASE_PASS='admin123'

`source /etc/profile` this make the variable become permanent
copy and paste the code below, to install neccesary packages to run our service


```
sudo -i
yum update -y
yum epel-release -y
yum install git mariadb-server -y
systemctl start mariadb
systemctl enable mariadb
systemctl statutus mariadb
mysql_secure_installation
#folllow all the command prompts and configure the database

```
once you are done with this run `mysql -u root -p` enter your password and you are in, now exit `exit`

we will run  `git clone ` to fetch our source code so as to initialize our database







once you are done log out with `logout `

### 2. setup memcached

`vagrant ssh mc01`

copy

```
sudo -i
yum update -y

yum install epel-release -y
```

to validate `ss -tunlp | grep 11211`

`exit`

### 3. setup rabbitmq
`vagrant ssh rmq01`
```
sudo -i
yum update -y


```

### 4. setting up application server

login to our app server `vagrant ssh app01`update packages, install dependencies jdk and some tools like wget and git
copy and paste the code below to 

```
#switch to root user
sudo -i

yum update -y
yum install epel-release -y




### 5. setting up web server


we are in the final configuration, 
log in to web server `vagrant ssh web01`

```
sudo apt update && sudo apt upgrade -y
sudo -i 
apt install nginx -y
vim /etc/nginx/sites-available/vproapp

```




# part 2: Automate the Deployment of a multi-tier application Locally

we will create all neccesary settings to start up and deploy the app and provision it in the vagrant file
`vim 
`vim Vagrantfile`
























