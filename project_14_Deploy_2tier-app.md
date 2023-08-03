
# Deploying a 2 tier architechture on AWS (manually)


* In this project, we will be working with the web tier and the application tier to safely secure 
 our application,

1. ***VPC***
  - What is a vpc
   Amazon Virtual Private Cloud (Amazon VPC) is a service offered by Amazon Web Services (AWS) that
   allows you to create a logically isolated section of the AWS Cloud where you can launch AWS
   resources in a virtual network that you define.
   This gives you control over your network environment, including IP address ranges,
   subnets, route tables, and network gateways.
   This component will basically house all other services we will be using in this project

   - we will create our custom vpc in ohio region for this project

     ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/vpc.PNG)

2. ***SUBNETTING***

  - Within a VPC, you can create multiple subnets. Subnets are segments of the IP address
    range of your VPC, and they can be either public or private

   lets create 4 subnets of which will consist of 2 private subnets and
   2 public subnet in 2 different availability zones, the will ensure high availability 

   ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/sub.PNG)

4. ***SECURITY GROUPS***
  - create security groups for:

   ***a- jumpserver***
   RULES
    - port : 22
      from: anywhere
            
   ***b- webserver***
   RULES
    - port : 22
      from: <jumpserver_private_ip>

    - port : 80
      from : anywhere
      
***4. KEYPAIRS***

   - create 2 seperate key pairs for both instances
   - jumserver: jump-key.pem
   - webserver: web-key.pem

5.   ***INSTANCES***
    Launch instances in all the subnets
   - jumpserver : ubuntu OS
   - webserver : centOS
  
    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/webserver.PNG)

5. ***create internet gateway***

   - when we create our internet gateway, we will attach it to the vpc we created.
     By attaching an Internet Gateway to oour VPC, it enables resources within our VPC,
     such as Amazon EC2 instances, to communicate directly with the internet.
    
    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/igw.PNG)

6. ***NAT gateways***

   -  In a VPC, instances located in private subnets don't have a direct route to
      the internet through an Internet Gateway. To enable these instances to access
      the internet while still maintaining security, you use a NAT Gateway.
   -  we will create a NAT gateway for our project, note NAT gatway is not free
      do not forget  to delete it imediately after the completion of this project
      
       ![]()

   
7. ***ROUTE TABLES***
  
  - let create public and private route table
     route table is essentially a set of rules (routes) that determine where network
     traffic should be directed. It acts as a map that helps network traffic find
     its way from source to destination. 

    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/a.PNG)



8. Associate subnets with route tables
    associate the public subnets to the internet gateway 
    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/Capture.PNG)

    and also do same for private subnets

    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/Capture.PNG)

    define routes 

   ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/routes.PNG)

9. login to the jump server

    - Now lets log in to our jump server

    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/jumplogin.PNG)

11. copy web server login key to the jump server machine

    ```
    scp -i <jumpserverkeyname> <webserverkeyname> <destination>
    ```
   ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/scp.PNG)

11. login to web server from the jump server
    open gitbash and ssh in to the jump server to verify its working
    perfectly
    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/weblogin.PNG)

12. ***Run the script***

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

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/httpd.PNG)

13. validate

check the httpd is active
![](https://github.com/baraqheart/HandsOn/blob/main/project_14/active.PNG)

goto to your web browser and validate that web page is loaded


