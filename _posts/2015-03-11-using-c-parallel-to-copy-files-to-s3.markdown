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






**Using Parallell.ForEach to list all directories**

* Code:

<pre>
<code>
          static void ReplicationFilesRecursive(string localDir, BAmazonS3 pBAmazonS3, string cleanPath = null)
        {
            Parallel.ForEach(Directory.GetDirectories(localDir), dirPath =>
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
            });
        }
</code>
</pre>


* Results:


![Perfomance Case 2](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Result2.png?raw=true"Perfomance Case 2")





**Using Parallell.ForEach to list all directories and images**

* Code:

<pre>
<code>
          static void ReplicationFilesRecursive(string localDir, BAmazonS3 pBAmazonS3, string cleanPath = null)
        {
            Parallel.ForEach(Directory.GetDirectories(localDir), dirPath =>
            {
                string currentFolder = Path.GetFileName(dirPath);
                string currentKey = cleanPath != null ? dirPath.Replace(cleanPath, string.Empty).Replace(@"\",
                                    "/") : dirPath.Replace(@"\", "/");

                Console.WriteLine(string.Format("Diretorio {0} replicado", currentFolder));
                Parallel.ForEach(Directory.GetFiles(dirPath), filePath =>
                {
                    string currentFile = Path.GetFileName(filePath);
                    using (Stream fileStream = File.Open(filePath, FileMode.Open))
                    {
                        pBAmazonS3.SaveObject(fileStream, string.Format(@"{0}/{1}", currentKey,
                        currentFile));
                        Console.WriteLine(string.Format("Arquivo {0} replicado",
                        Path.GetFileName(filePath)));
                    }
                });
                ReplicationFilesRecursive(dirPath, pBAmazonS3, cleanPath);
            });
        }

</code>
</pre>


* Results:


![Perfomance Case 3](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Result3.png?raw=true"Perfomance Case 3")




**Comparing the results**

![Summary](https://github.com/aragostinho/CopyFasterToS3/blob/master/images/Summary.PNG?raw=true"Perfomance Case 3")




**Conclusion**

The use of Parallel was possible in this case because the copy of files can be executed in many threads without locked or process concurrence. Each iteration opened and release a stream file to be transfer in *BAmazonS3()* class. After that BAmazonS3() class just send the stream to S3 Buckets using the *SaveObjectMethod()*.   The gain of performance was 900% in the first case using parallelism in files iteration, and 946% in the second case using parallelism in files iteration. The gain in the second case could be higher, but in my scenario each folder had 5 files on average.