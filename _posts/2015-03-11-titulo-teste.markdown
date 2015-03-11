---
published: true
title: Using C# Parallel to copy files to S3  	
layout: post
---
**Introduction**
If you started to work with AWS services and need to transfer your files to S3 buckets and your company has large directories with thousands or millions files, this article can be useful to you.

**A simple scenario**
A folder named *CompanyFiles*  that contains  17.917 Sub Folders and 45.090 Files. Total of 181Mb.
I need to copy these files to a s3 bucket  called  *bucket-replication*.


**About the solution**
In this article i will explain a simple solution based in a recursive function and after that i will discuss how to optimize the code to achieve the goal efficiently. You can view the solution in [https://github.com/aragostinho/Cervejando](https://github.com/aragostinho/CopyFasterToS3)