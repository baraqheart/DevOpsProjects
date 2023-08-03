
# Deploying a 2 tier architechture on AWS (manually)

* In this project, we will be working with the web tier and the application tier to safely secure 
 our application,

1. create a custom VPC

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/vpc.PNG)

2. 4 subnets, 2 private subnets and 2 public subnet in 2 different availability zones,
   the will ensure high availability 

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/sub.PNG)

3. create security groups for:
   a- jumpserver
   b- webserver
   c- load balancer

   

4. create key pair for 
   a- jumserver: jump-key.pem
   b- webserver: web-key.pem
5. Launch instances in all the subnets
   - jumpserver
   - webserver
  
    ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/webserver.PNG)

5. create internet gateway

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/igw.PNG)

6. create NAT gateways
![]()

![]()
7. create public route table  

![]()

8. create private route table

![]()

9. Associate subnets with route tables
associate the public subnets to the internet gateway 
![](https://github.com/baraqheart/HandsOn/blob/main/project_14/Capture.PNG)

and also do same for private subnets
![](https://github.com/baraqheart/HandsOn/blob/main/project_14/Capture.PNG)

define routes 

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/routes.PNG)

10. login to the jump server

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/jumplogin.PNG)

11. copy web server login key to the jump server machine

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/scp.PNG)

12. login to web server from the jump server

 ![](https://github.com/baraqheart/HandsOn/blob/main/project_14/weblogin.PNG)
12. Run the script

```

```

![](https://github.com/baraqheart/HandsOn/blob/main/project_14/httpd.PNG)

13. validate

check the httpd is active
![](https://github.com/baraqheart/HandsOn/blob/main/project_14/active.PNG)



