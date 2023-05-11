# lift and shift project
### flow of execution (steps)
1. login into AWs account
2. create a key pair
3. create security groups 
4. launch 4 instance with user data
5. update ip to the name mapping in route 53
6. build the artifact from the source code on your local machine
7. upload to s3 bucket
8. download artifact to tomcat server ec2 instance
9. setup elb with https with aws certificate manager
10. map elb endpoint to website name in godaddy dns
11. verify
12. build autoscalling group for tomcat instance


### 1.
create aws account or log in if you have one

### 2. go to ec2 and select keypair and create one

tag:
 - name: vprofile-prod-key.pem
 

### 3. start creating security groups 

a) SG for load balancer
name = elb-sg
port 
80	 from anywhere
143	from anywhere

b) SG for tomcat  
name = app-sg
port
22		from-my-ip
8080		elb-sg 

c) SG for backend
name = backend-sg

port
22		from-my-ip
3306		from-app-sg
11211		from-app-sg
5672		from-app-sg
all-trafic	from-itself



### 4. we will launch our instances

a)we will launch an instance for our database
choose cent0s7
instance-type: t2.micro

a)we will launch an instance for our cache
name: mc01
choose cent0s7
instance-type: t2.micro

a)we will launch an instance for our message broker

name: rmq01
choose cent0s7
instance-type: t2.micro

and tick protect against accidental termination for all these instances 

try ` ss -tunpl | grep <port> `
to check if the the processes are running on the right port

`ps -ef` shows the process and the port the are running on

### 5. map ip to name on route 53
put down  dbo1, mq01 and rmq01 private address down to be used for mapping

db01	<private_ip_address>
mc01	<private_ip_address>
rmq01	<private_ip_address> 

### step 6: build artifact locally 

install jdk8 on your local machine, using windows  os


` choco install jdk8 -y `

` choco install maven -y `

navigate into the application.resources file replace 
rmq01 as rmq01.vprofile.in
mc01 as mc01.vprofile.in
db01 as db01.vprofile.in

these are the route 53 dns zone entries
save and quit and go back to the top level dir of the vprofile dir

run `mvn install`

our artifact is ready

#7. upload artifact to s3 bucket 
we will use aws cli to perform this operation 
we need to install aws cli

windows os
run ` choco install awscli -y

create an iam user to authenticate awscli
attach programmatic access
give existing policy and attach s3 bucket full access <do not expose this user access and secret keys>


now we will authenticate from the terminal

`aws configure` and it will prompt you for the following
Access key ID:
Secret access key:
Default aws region
Default output format: json

now lets make a bucket
`aws s3 mb <s3://name_of_bucket>

copy artifact to s3 bucket
`aws s3 cp <artifact_name> <s3://name_of_bucket>`

to check contents of bucket
`aws s3 ls <s3://name_of_bucket>`

lets create an Iam role to allow ec2 access s3 bucket

create an iam role 
name: 

attach this iam role to our appserver

login to our app server and validate tomcat8 is working
`sytemctl status tomcat8`

`cd var/lib/tomcat8`  
`ls`
`cd webapps
ls

we need awscli 
`apt install aws cli`

`aws s3 ls <s3://name_of_bucket>`

lets download to ec2

`aws s3 cp <s3://name_of_bucket>/<filename> /tmp/<filename>`


now lets copy this artifact to /webapps directory renaming it as ROO.war

start tomcat

`systemctl start tomcat8`

`ls ROOT` and `cd WEB-INF` then `ls` now you can `cd classes` and `ls`
finally you will locate the application.properties file

lets `cat` the file to see the contents

now lets validate network connectivivity
`telnet db01 3306`

this should output connected if successful


8. Load balancer

to use load balancer we need to create target group 

select app server as the only 





