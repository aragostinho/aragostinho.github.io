---
published: true
title: Using C# Parallel to copy files to S3 
layout: post
---
**Introduction**

If you started to work with AWS services and need to transfer your files to S3 buckets in a solution that has many and large directories with thousands or millions files, this article can be useful to you.
In this article i'll show and provide a simple solution based in a recursive function, after that i will demonstrate two code changes that will increase the performance during the execution plan achieving the goal efficiently. Finally, you can get the solution  [CopyFasterToS3](https://github.com/aragostinho/CopyFasterToS3). Enjoy it  :)


**A simple scenario**

A folder named *CompanyFiles*  that contains  17.917 Sub Folders and 45.090 Files. Total of 181Mb.
I need to copy these files to a s3 bucket  called  *bucket-replication*.


**Baby Steps**

* Download the [Amazon SDK for .Net](https://www.nuget.org/packages/AWSSDK) from Nuget
* Code a basic class with methods like *SaveFile()*, *DeleteFile()*, *ListFiles()*, etc.  If you have doubts how to do this, you can download  [S3CmdInCSharp](https://github.com/aragostinho/S3CmdInCSharp)
* Code a simple recursive function to search all folders, subfolders and files. In this solution a used a *tail recursion*. In a [tail recursion](https://www.cs.cmu.edu/~adamchik/15-121/lectures/Recursions/recursions.html) , basically the functions is invoked in the end, after to execute all statements.






**Using a simples foreach**

* Code:
<script src="https://gist.github.com/aragostinho/d7e18f5bf17f63f44d68.js"></script>



* Results:


![Perfomance Case 1](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Result1.png?raw=true"Perfomance Case 1")






**Using Parallell.ForEach to list all directories**

* Code:
<script src="https://gist.github.com/aragostinho/e14a90f283137ab57dec.js"></script>


* Results:


![Perfomance Case 2](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Result2.png?raw=true"Perfomance Case 2")





**Using Parallell.ForEach to list all directories and images**

* Code:
<script src="https://gist.github.com/aragostinho/b61171582c8e9bebd1b8.js"></script>

* Results:


![Perfomance Case 3](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Result3.png?raw=true"Perfomance Case 3")




**Comparing results**

![Summary](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Summary.PNG?raw=true"Perfomance Case 3")




**Conclusion**

The use of Parallel was possible in this case because the copy of files can be executed in many threads without locked or process concurrence. Each iteration opened and release a stream file to be transfer in *BAmazonS3()* class. After that BAmazonS3() class just send the stream to S3 Buckets using the *SaveObjectMethod()*.   The gain of performance was 900% in the first case using parallelism in files iteration, and 946% in the second case using parallelism in files iteration. The gain in the second case could be higher, but in my scenario each folder had 5 files on average.
