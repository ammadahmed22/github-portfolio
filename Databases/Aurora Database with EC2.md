<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Aurora Database with EC2

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-aurora)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

## Connect a Web App to Amazon Aurora

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-aurora_44443546)

---

## Introducing Today's Project!

### What is Amazon Aurora?

Amazon Aurora is an AWS relational database service that is best suited for large scale database that require high performance and uptime.

### How I used Amazon Aurora in this project

I used Amazon Aurora in today's project by creating an Amazon Aurora database, setting up an EC2 instance  (web server) separately, and then connecting that Aurora database to our EC2 instance. 

### One thing I didn't expect in this project was...

One thing I didn't expect was that we had to create our EC2 instance first before finishing setting up our Amazon Aurora database. 

### This project took me...

This project took me around 1 hour due to being confused about dbs.t3.medium.

---

## In the first part of my project...

### Creating an Aurora Cluster

A relational database is a type of database that organizes data into tables, which are collections of rows and columns. Kind of like a spreadhsheet! We call it "relational" because the rows relate to the columns and vice-versa.

Aurora is a good choice when your application demands a large-scale relational database solution that delivers peak performance and high uptime. It's particularly well-suited for "big jobs" and demanding workloads where standard relational databases might struggle. This is because Aurora is designed with features like database clusters that enhance its performance, scalability, and availability, making it ideal for critical applications that require consistent, high-level operation, such as managing large volumes of consumer data like Coca-Cola does. While other relational databases like generic MySQL or Oracle can be more cost-effective for smaller databases or less intensive tasks, Aurora shines when the requirements for scale and reliability are significantly higher.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-aurora_44443546)

---

## Halfway through I stopped!

I stopped creating my Aurora database because I realized I hadn't yet created the EC2 instance that the database needs to connect to.

The setup process reached a point where I needed to select an EC2 instance to associate with the Aurora database (for the "Connect to an EC2 compute resource" option). However, since that EC2 instance doesn't exist yet, I needed to pause the database creation, go create the EC2 instance first, and then I can come back to complete the Aurora database setup with the correct EC2 connection.

### Features of my EC2 instance

I created a new key pair for my EC2 instance because this key pair provides the secure credentials necessary to access and manage the instance.

Essentially, the key pair acts like a secure login password for this virtual server. Without it, I wouldn't be able to connect to the EC2 instance to perform crucial tasks such as installing software, deploying the web application files, or making any configuration changes to how the instance is running. Creating a new key pair ensures I have the unique "keys" needed to securely control this specific EC2 instance.

When I created my EC2 instance, I took particular note of its Public IPv4 DNS address and the name of the key pair associated with it. These details are critical because, as the analogy suggests, the Public IPv4 DNS is like the address or location of my virtual server; it tells me where to connect. The key pair, on the other hand, provides the secure "keys" or credentials required to actually get into and access that server. Without the correct address, I wouldn't know where to direct my connection, and without the correct key pair, I wouldn't be authorized to log in, even if I found the server. Both are therefore essential for successfully connecting to and managing the EC2 instance.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-aurora_91b9fd1g)

---

## Then I could finish setting up my database

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-aurora_1fddb0b5)

Aurora Database uses clusters because they provide high availability, fault tolerance, and enhanced performance, which are essential for large-scale and demanding applications. A cluster in Aurora consists of a primary "writer" instance and one or more "reader" replica instances. This architecture means that if the primary instance fails, one of the read replicas can be automatically promoted to become the new primary, ensuring your data remains continuously available and minimizing downtime. Furthermore, by distributing read operations across multiple reader instances, clusters can handle a higher volume of read requests and improve overall database performance, especially as the workload grows. This separation also allows the writer instance to focus efficiently on data modification tasks. Essentially, clusters are why Aurora is well-suited for "big jobs" requiring robust, always-on database operations.

---

---
