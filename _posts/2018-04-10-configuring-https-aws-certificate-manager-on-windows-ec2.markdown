---
published: false
title: Configuring HTTPS on Windows EC2 with AWS Certificate Manager
description: Step-by-Step to setting up HTTPS protocol on Windows EC2 server using AWS Certificate Manager
layout: post
---


## Introduction

The service Certificate Manager from Amazon Web Services (AWS) is a very useful service to manage certificates without any concern about expirated date or maintenance.
You can setup a certificate in few minutes and use it on AWS Resource, for example a EC2 through a Load Balance. The benefits are reducing setup time, low certificate costs,
remove any maintanence responsability and an excelent certificate quality (you can check using [Qualys SSL Labs](https://www.ssllabs.com/)).
This article show step-by-step everything you need to know for configure HTTPS on a EC2 running Windows Server 2016 using a SSL certificate from AWS Certificate Manager.


## Table of Contents

1. [Configuring Certificate Manager on AWS](#1-configuring-certificate-manager-on-aws)
2. [Creating an EC2 server running Windows Server 2016](#2-configuring-certificate-manager-on-aws)
3. [Creating and configuring an Application Load Balancer](#3-creating-and-configuring-an-application-load-balancer)
4. [Configuring HTTPS protocol on EC2 using IIS](#4-configuruing-https-protocol-on-ec2-using-iis)
5. [Configuring DNS zone using Route 53](#5-configuring-dns-zone-using-route-53)
6. [Application Ajustements](#6-application-adjustements)
7. [SEO Questions](#7-seo-questions)
8. [Conclusion](#8-conclusion)


## 1) Configuring Certificate Manager on AWS

The first step is setup a certificate on AWS Certificate Manager (ACM), so if you don't have an AWS account create it before following this step. It's possible using ACM in:

• Elastic Load Balancing 
• Amazon CloudFront
• Amazon API Gateway
• AWS Elastic Beanstalk
• AWS CloudFormation

### 1.1) Requesting a public certificate
Select "Get Started" for provision certificates. By default, public certificates are trusted by browsers and operating systems.

![Creating a public certificate](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image1.PNG?raw=true)

### 1.2) Entering the domain that will use the certificate

![Defining domains](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image2.PNG?raw=true)

### 1.3) (Optional) Enter the subdomain that will use the certificate
If you need to use the same certificate to subdomain for example ***anything**.aws.com* it's necessary to define each subdomain during the creation process. **It's not possible to change it after certificate created**. If you need more than on subdomain and there are needs to scale for future subdomain it's recommended to use a wildcard.

![Defining subdomains](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image3.PNG?raw=true)


### 1.4) Selecting a validation option
There are two possibilies for validate the request: Email or DNS. In this guide I will choose  Email option because it's easier than DNS option, but if you choose DNS, you need to configure some entries like CName. This configuration it's possible in Route 53 [Learn How Validate From DNS](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate-dns.html). 

**IMPORTANT:** Verify if you have access to manage emails accounts like *administrator@domain, webmaster@domain, postmaster@domain, hostmaster@domain* or an email account in your AWS Account. It's very important because if you don't have, the e-mail verification can't be delivery and the validation proccess will be stuck. It's very boring and frustating.


![Select a validation option](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image4.PNG?raw=true)


### 1.5) Confirming all information
Pay attention before confirm the information because you can change anything after certificate created (just delete and recreate).


![Confirm all information](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image5.PNG?raw=true)


### 1.6) Sending the verification e-mail
How mentioned in the step 1.4 the verification email will send to following emails accounts acording of your domain and AWS Account configuration. 

 ![Sending verification email](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image6.PNG?raw=true)

### 1.7) Receiving the verification e-mail
If everything was right you must receive an e-mail with a link for validate the certificate. 

 ![Receiving verification email](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image7.PNG?raw=true)
 
 
### 1.8) Confirming the certificate
Just confirm you SSL/TLS certificate and congrats you have an AWS Certificate now! 

 ![Confirming the certificate](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image8.PNG?raw=true)
 
    
 
## 2) Creating an EC2 server running Windows Server 2016
If you have a EC2 server configured and running Windows Server 2016 please skip this topic.


### 2.1) Choosing an AMI 
Choose Windows 2016 Server 64 Bits Base AMI

 ![Creating an EC2 From AMI](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image10.PNG?raw=true)

### 2.2) Choosing an Instance Type

Choose any EC2 Instance Type and of course, pay attention about [prices and differences each one](https://www.ec2instances.info/).  In this guide a T2.Micro instance type AMI will be use.
  
 ![Choosing an Instance Type](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image11.PNG?raw=true)


### 2.3) Configuring Instance Details
In this step there are a lot of itens to configuration but we will just focus on minimium and necessary itens like Network, SubtNet and Autosign IP. Just keep the default network and choose avaible subnet for the following Network. It's important to keep checked auto-assign public IP because the EC2 needs to be public for the Load Balancer.
    
 ![Configuring Instance Details](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image12.PNG?raw=true)


### 2.4) Adding Storage
A simple and cheap magnetic disk it's enough for small apps, but I really recommend SSD disks from sketch even if this is your scenario. The reason? It's painfull [changing EBS disk type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/limitations.html) after EC2 is in production. 


 ![Adding Storage](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image13.PNG?raw=true)

### 2.5) Defining Key Tags
It's a good practice for organizing resources in AWS if you keep in mind find and manage quickly each resource.

 ![Defining Keys](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image14.PNG?raw=true)

### 2.6) Configuring Security Group
I recommend create a new security group, define a good name e additional information. When AWS usage grows up for example EC2s, it's important to know what kind of Security Group is responsible for.  

It's necessary to open the follow ports:
 **Port 80** - Because it's a web server and you need the port for Load Balancer communitcation
 **Port 3389** - To grant access for Remote Desktop Windows.


 ![Configuring Security Group](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image15.PNG?raw=true)
 
### 2.7) Creating Key pairs
Finally, it's necessary to create or choose existing key pairs for granting access to EC2. *Please keep safe the keys!!*
  
  ![Creating Key pairs](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image16.PNG?raw=true)
 
### 2.8) Running Ec2
After the creation steps verify if the Ec2 is running well and test a remote access using Microsoft Remote Desktop. If you don't know how do it please read this article.
 
   ![Running Ec2](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image17.PNG?raw=t)
 
## 3) Creating and configuring an Application Load Balancer
Why we must have to create a Load Balancer? The answer is for using with a AWS Certificate (from AWS Certificate Manager). 
But, is it possible to "attach" the AWS Certificate directly on EC2? No! It's not possible. When you create a Load Balancer with ACM it ables a cryptografied connection for the whole traffic, this security layer (let's say it) will be use between EC2 and DNS Zone. This is another big gain of productivity because there is no need to setup a certificate in OS (ex: Windows).

**In AWS there are three options for Load Balancer:**

• **Application Load Balancer:** Choose an Application Load Balancer when you need a flexible feature set for your web applications with HTTP and HTTPS traffic. Operating at the request level, Application Load Balancers provide advanced routing, TLS termination and visibility features targeted at application architectures, including microservices and containers.

• **Network Load Balancer:** Choose a Network Load Balancer when you need ultra-high performance and static IP addresses for your application. Operating at the connection level, Network Load Balancers are capable of handling millions of requests per second while maintaining ultra-low latencies.

• **Classic Load Balancer (depreceated)** (previously Load Balancer. Nowadays you can use Application Load Balancer)

In this guide the Application Load Balancer will be choose.


   ![Application Load Balancer](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image19.PNG?raw=t)


## 4) Configuring HTTPS protocol on EC2 using IIS

## 5) Configuring DNS zone using Route 53

## 6) Application Ajustements

## 7) SEO Questions
 
## 8) Conclusion






 



