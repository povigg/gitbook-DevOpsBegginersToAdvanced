---
description: Will setup vprofile test project using AWS infrastructure
---

# AWS Project setup

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Project plan

1. Create key pairs for SSH to EC2 instances
2.  Create security groups for Load balancer, Tomcat Instances and backend services.

    Load balancer SG wilL be able to access > Tomcat instances SG. Tomcat SG will be able to access Backend services SG
3. Launch EC2 instances with user data (bash scripts)
4. Update DNS mapping in Route53 service
5. Build an app from source code (will do this on local machine)
6. Upload artifacts to S3 bucket
7. Download artifacts from S3 to EC2 instances
8. Setuo ELB (load balancer) with HTTPS. Cert from Amazon certificate manager
9. Map ELB endpoint to website name in Godaddy DNS
10. Verify

### Security groups

First SG will be for load balancer. We should be able to access LB using HTTP and HTTPS ports from anywhere. (HTTP will be removed after full project setup)

Application SG: Tomcat service will bne using port 8080, so we need to give and access for this port  **FROM load balancer SG** only

Last SG is for Backend services. MYSQL using port 3306, memcached port 11211, rabbit mq port 5672. Allow **application SG as a source only.** Add extra rule within SG for **All traffic from Itself**. (Backend services need to communicate in between)

### EC2 Instances

#### Backend instances:

* **EC2 instance for DB**. Will be ryun ning centos7, using backend SG, use created keypair and add user data (bash script)
* Add SSH inbound rule from my IP to backend SG
* **EC2 Instance for Memcache**. Centos7, backend SG, use created keypair, add user data
* E**C2 instance for Rabbit MQ**, centos7, backend SG, use created keypair, add user data

### Route53

* Create backen zone (private) for backend servers and mapp IP addresses

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

*