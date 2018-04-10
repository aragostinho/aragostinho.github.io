---
published: false
title: Configuring HTTPS on Windows EC2 with AWS Certificate Manager
description: Step-by-Step to setting up HTTPS protocol on Windows EC2 server using AWS Certificate Manager
layout: post
---
**Introduction**

The service Certificate Manager from Amazon Web Services (AWS) is a very useful service to manage certificates without any concern about expirated date or maintenance.
You can setup a certificate in few minutes and use it on AWS Resource, for example a EC2 through a Load Balance. The benefits are reducing setup time, low certificate costs,
remove any maintanence responsability and an excelent certificate quality (you can check using [Qualys SSL Labs](https://www.ssllabs.com/)).
This article show step-by-step everything you need to know for configure HTTPS on a EC2 running Windows Server 2016 using a SSL certificate from AWS Certificate Manager.


**1) Configuring Certificate Manager on AWS**

The first step is setup a certificate on AWS Certificate Manager, so if you don't have a AWS account create it before following this step.

 


