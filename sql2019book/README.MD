
![Book Graphic](./graphics/sql2019bookimage.jpg)

# SQL Server 2019 Revealed

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2><img style="float: left; margin: 0px 15px 15px 0px;" src="https://github.com/microsoft/sqlworkshops/blob/master/graphics/textbubble.png?raw=true"><b>     About this Book</b></h2>

Get up to speed on the game-changing developments in SQL Server 2019. No longer just a database engine, SQL Server 2019 is cutting edge with support for machine learning (ML), big data analytics, Linux, containers, Kubernetes, Java, and data virtualization to Azure. This is not a book on traditional database administration for SQL Server. It focuses on all that is new for one of the most successful modernized data platforms in the industry. It is a book for data professionals who already know the fundamentals of SQL Server and want to up their game by building their skills in some of the hottest new areas in technology.

***SQL Server 2019 Revealed*** begins with a look at the project's team goal to integrate the world of big data with SQL Server into a major product release. The book then dives into the details of key new capabilities in SQL Server 2019 using a “learn by example” approach for Intelligent Performance, security, mission-critical availability, and features for the modern developer. Also covered are enhancements to SQL Server 2019 for Linux and gain a comprehensive look at SQL Server using containers and Kubernetes clusters.
The book concludes by showing you how to virtualize your data access with Polybase to Oracle, MongoDB, Hadoop, and Azure, allowing you to reduce the need for expensive extract, transform, and load (ETL) applications. You will then learn how to take your knowledge of containers, Kubernetes, and Polybase to build a comprehensive solution called Big Data Clusters, which is a marquee feature of 2019. You will also learn how to gain access to Spark, SQL Server, and HDFS to build intelligence over your own data lake and deploy end-to-end machine learning applications.

### What You Will Learn

Implement Big Data Clusters with SQL Server, Spark, and HDFS
Create a Data Hub with connections to Oracle, Azure, Hadoop, and other sources
Combine SQL and Spark to build a machine learning platform for AI applications
Boost your performance with no application changes using Intelligent Performance
Increase security of your SQL Server through Secure Enclaves and Data Classification
Maximize database uptime through online indexing and Accelerated Database Recovery
Build new modern applications with Graph, ML Services, and T-SQL Extensibility with Java
Improve your ability to deploy SQL Server on Linux
Gain in-depth knowledge to run SQL Server with containers and Kubernetes
Know all the new database engine features for performance, usability, and diagnostics
Use the latest tools and methods to migrate your database to SQL Server 2019
Apply your knowledge of SQL Server 2019 to Azure


### Who This Book Is For

IT professionals and developers who understand the fundamentals of SQL Server and wish to focus on learning about the new, modern capabilities of SQL Server 2019. The book is for those who want to learn about SQL Server 2019 and the new Big Data Clusters and AI feature set, support for machine learning and Java, how to run SQL Server with containers and Kubernetes, and increased capabilities around Intelligent Performance, advanced security, and high availability. 

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2><img style="float: left; margin: 0px 15px 15px 0px;" src="https://github.com/microsoft/sqlworkshops/blob/master/graphics/checkmark.png?raw=true"><b>     Using the Examples</b></h2>

The examples for this book are available through scripts in folders on this GitHub repo. They are provided as a reference for you to use as a compliment while reading the book. You need to read the chapters of the book to see instructions, with example output, on how to use these examples. In some cases, example commands are provide *inline* in the book itself.

It is possible to use ALL the examples in this book on a SQL Server 2019 deployment on a laptop. The only exception is Chapter 8 where you need to have a Kubernetes cluster (The book uses Azure Kubernetes Service as an example k8s cluster).

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2><img style="float: left; margin: 0px 15px 15px 0px;" src="https://github.com/microsoft/sqlworkshops/blob/master/graphics/listcheck.png?raw=true"><b>     Additional Information for the Book</b></h2>

 Since the writing of the book there are few areas of clarification that you should know about organized by chapter.

### Chapter 3 New Security Capabilities

- Always Encrypted and Indexes

In this chapter a sentence says "Given that the client application is the only place where decryption takes place, there is no way to truly build an index with Always Encrypted". This is not the entire story. Thanks to Tom Norman from the community pointing out that the 100% accurate answer is that Always Encrypted prior to SQL Server 2019 did support indexes using deterministic encryption but not with randomized encryption. You can read more about ths in the documentation at https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver15#selecting--deterministic-or-randomized-encryption. Always Encrypted with secure enclaves talks about new indexing capabilities at https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#enclave-enabled-columns.

### Chapter 8 SQL Server on Kubernetes

- Using the correct StorageClass

The example for **storage.yaml** used an incorrect name for the StorageClass. The book and the example shows this.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mssql-data
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
```

This line

```yaml
storage-class: azure-disk
```

should be this

```yaml
storage-class: managed-premium
```

This has been corrected in the GitHub example scripts in the **ch8_sql_on_k8s** folder.

- Using non-root containers with k8s

SQL Server 2019 ships by default with non-root containers which was not used when the book was written. Therefore, the **sql2019deployment.yaml** file needs this additional line

```yaml
     securityContext:
        fsGroup: 1000
```
to allow the non-root container to access the PersistentVolumeClaim (PVC). The **sql2019deployment.yaml** file has been updated in the **ch8_sql_on_k8s** folder to include this.

### Chapter 9 Data Virtualization

- Requirements for columns with external tables

The book makes this statement "The number of columns and their data types must match exactly with the external data source table, but you can use any name you want on the SQL side both for the column names and the name of the table itself."

This was true in earlier preview releases of SQL Server 2019. However, in the General Availability release of SQL Searver 2019, the only requirement is the column name. It must match exactly to the target data source but the ordering does not matter. And, you can specify only a subset of columns from the data source.