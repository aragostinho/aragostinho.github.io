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

In this article i will explain a simple solution based in a recursive function and after that i will discuss how to optimize the code to achieve the goal efficiently. You can download the solution  [CopyFasterToS3](https://github.com/aragostinho/CopyFasterToS3)

**Setup**

* Download the last version of [Amazon SDK for .Net](https://www.nuget.org/packages/AWSSDK) from Nuget
* Codify a basic class with methods like *SaveFile()*, *DeleteFile()*, *ListFiles()*, etc.  If you have doubts how to do this, you can download  [S3CmdInCSharp](https://github.com/aragostinho/S3CmdInCSharp)


**Using a simples foreach**

* Code:

<pre>
<code>
  static void ReplicationFilesRecursive(string localDir, BAmazonS3 pBAmazonS3, string cleanPath = null)
        {
            foreach (string dirPath in Directory.GetDirectories(localDir))
            {
string currentFolder = Path.GetFileName(dirPath);
string currentKey = cleanPath != null ? dirPath.Replace(cleanPath, string.Empty).Replace(@"\", "/") : dirPath.Replace(@"\", "/");

                Console.WriteLine(string.Format("Diretorio {0} replicado", currentFolder));
                foreach (string filePath in Directory.GetFiles(dirPath))
                {
                    string currentFile = Path.GetFileName(filePath);
                    using (Stream fileStream = File.Open(filePath, FileMode.Open))
                    {
     pBAmazonS3.SaveObject(fileStream, string.Format(@"{0}/{1}", currentKey,    
     currentFile));
Console.WriteLine(string.Format("Arquivo {0} replicado", 
            Path.GetFileName(filePath)));
                    }
                }
                ReplicationFilesRecursive(dirPath, pBAmazonS3, cleanPath);
            }
        } 

</code>
</pre>


* Results:


![Perfomance Case 1](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Result1.png?raw=true"Perfomance Case 1")