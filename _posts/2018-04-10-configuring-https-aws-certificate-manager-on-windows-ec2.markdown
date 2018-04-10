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


## 1) Configuring Certificate Manager on AWS

The first step is setup a certificate on AWS Certificate Manager, so if you don't have an AWS account create it before following this step.

### 1.1) Request a public certificate
Select "Get Started" for provision certificates. By default, public certificates are trusted by browsers and operating systems.

![Creating a public certificate](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image1.PNG?raw=true)

### 1.2) Enter the domain that will use the certificate

![Defining domains](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image2.PNG?raw=true)

### 1.3) (Optional) Enter the subdomain that will use the certificate
If you need to use the same certificate to subdomain for example ***anything**.aws.com* it's necessary to define each subdomain during the creation process. **It's not possible to change it after certificate created**. If you need more than on subdomain and there are needs to scale for future subdomain it's recommended a wildcard like defination ***.aws.com*.

![Defining subdomains](https://github.com/aragostinho/aragostinho.github.io/blob/master/_imgs/https/Image3.PNG?raw=true)



## 2) Validating the fresh certificate

Nononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo.


Nononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo.


## 3) Create an EC2 server running Windows Server 2016


Nononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo.


Nononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo.

Nononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo ononononono ononononononoo.

## 4) Create and configuring an Application Load Balance

## 5) Testing LoadBalance

## 6) Configuring HTTPS protocol on Ec2 using IIS

## 7) Configuring DNS zone using Route 53

**8) Application Ajustements**

**9) SEO Questions**

**10) Final tests**








 


