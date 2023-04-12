
# Hosting a static website on s3 

Amazon S3 (Simple Storage Service) is a highly scalable object storage service that allows you to store and retrieve data from anywhere on the web. It provides developers and IT teams with secure, durable, and highly available object storage. Here are some key features of Amazon S3:
1.	***Scalability***: Amazon S3 can handle any amount of data and can scale to support growing workloads. It also offers automatic scaling of storage resources to meet your needs.
2.	***Durability and availability***: Amazon S3 is designed to provide 99.999999999% (11 9's) durability and 99.99% availability of objects over a given year, making it an ideal choice for storing critical data.
3.	***Security***: Amazon S3 supports encryption at rest and in transit, and integrates with AWS Identity and Access Management (IAM) to provide fine-grained access control to objects.
4.	***Integration with other AWS services***: Amazon S3 integrates with many other AWS services, such as Amazon EC2, Amazon EMR, and Amazon CloudFront, making it easy to use in your AWS architecture.

### Steps

- create as s3 bucket 
- unblock all access since we want to access it via internet
![block](https://github.com/baraqheart/HandsOn/blob/aeb77a4108741a38fff34e865dc11d53bd707400/project_5/s1.PNG)


- uploads your website files 

![uploads](https://github.com/baraqheart/HandsOn/blob/aeb77a4108741a38fff34e865dc11d53bd707400/project_5/s2.PNG)


- confirm uploads is successful
 ![conf](https://github.com/baraqheart/HandsOn/blob/aeb77a4108741a38fff34e865dc11d53bd707400/project_5/s3.PNG)


-go to properties and enable a static site

![enable](https://github.com/baraqheart/HandsOn/blob/aeb77a4108741a38fff34e865dc11d53bd707400/project_5/s4.PNG)

![enabled](https://github.com/baraqheart/HandsOn/blob/aeb77a4108741a38fff34e865dc11d53bd707400/project_5/s5.PNG)


- the url is thrown an error page
![err](https://github.com/baraqheart/HandsOn/blob/aeb77a4108741a38fff34e865dc11d53bd707400/project_5/s6.PNG)

- and its working now because and ticked make it public
![working](https://github.com/baraqheart/HandsOn/blob/3667f706552297083116cac488f5274c5fbdf176/project_5/s7.PNG)

It's not necessarily that hosting a static site on Amazon S3 is inherently cost-ineffective, but rather that it may become more expensive than alternative options depending on the traffic and usage of the site.

Here are some reasons why hosting a static site on S3 may not be cost-effective in certain scenarios:

Request-based pricing: Amazon S3 charges for each HTTP request made to the site. If the site experiences high traffic or frequent requests, these charges can quickly add up and become more expensive than alternative options that charge a flat rate for hosting.

Data transfer fees: Amazon S3 also charges for data transfer out of the bucket. If the site has a large number of visitors or serves large files, these transfer fees can also add up quickly.

Lack of caching options: S3 does not offer the same caching options as a dedicated CDN (Content Delivery Network). This means that visitors may experience slower load times and increased data transfer fees as a result.

Overall, S3 can be a cost-effective option for hosting static sites with low traffic and data transfer requirements. However, if the site has high traffic or frequent requests, alternative hosting options such as a dedicated CDN or a web hosting service with a flat rate may be more cost-effective in the long run.
