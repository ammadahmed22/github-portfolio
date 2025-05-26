<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Connect a Web App with Aurora

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-webapp)

**Author:** Ammad Ahmed  
**Email:** ayaanahmedcoding@gmail.com

---

## Connect a Web App to Amazon Aurora

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-webapp_1709b26b)

---

## Introducing Today's Project!

### What is Amazon Aurora?

Amazon Aurora is a relational database service offered by AWS, designed for high performance and scalability, compatible with MySQL and PostgreSQL. It's not just a standard database; Aurora uses a distributed, fault-tolerant, and self-healing storage system built for the cloud that is decoupled from the compute instances. This architecture makes it particularly useful for applications demanding high availability, durability, and speed. Aurora is a good choice for large-scale operations and "big jobs" because it utilizes database clusters. These clusters typically consist of a primary "writer" instance and multiple "reader" instances (replicas). This setup ensures that data is always available, as one of the replicas can be automatically promoted if the primary instance fails. It also enhances performance by allowing read traffic to be distributed across the reader instances, making it ideal for demanding workloads requiring consistent uptime and fast query processing.

### How I used Amazon Aurora in this project

In the project focused on connecting a web app with Aurora, I used an Amazon Aurora (MySQL compatible) database as the backend data store for a web application hosted on an EC2 instance. First, I created an Aurora database cluster. Then, on the EC2 instance, I set up the web application environment, which included installing PHP and MySQL client libraries (MariaDB). A crucial part was creating a configuration file (dbinfo.inc) within the web application. This file stored the connection details—specifically the Aurora database's writer endpoint, username, password, and database name ('sample'). The web application (SamplePage.php) then used these details to connect to the Aurora database. Through this connection, the web app could create an EMPLOYEES table if it didn't exist, add new employee records submitted via a web form, and display all employee records from the database on the web page.

### One thing I didn't expect in this project was...

One aspect of this project that might have been less expected, especially for someone newer to managed database services like Aurora, could be the inherent complexity and architecture of an Aurora database cluster compared to a traditional single-instance database. The introduction of concepts like distinct "writer" and "reader" instances within the cluster, each with its own endpoint (though we primarily used the writer endpoint for the web app), and understanding how these contribute to high availability and read scaling, adds a layer beyond just setting up a simple database. While powerful, grasping how these components work together and why Aurora is structured this way for performance and resilience might have been a more involved learning point than just connecting to a database. The project's explanation of clusters and how they make Aurora suitable for "big jobs" highlights this as a key, and potentially new, concept.

### This project took me...

This project took approximately 1.5 hours to complete, which also included some additional time for exploring and experimenting with the setup at the end.

---

## Creating a Web App

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-webapp_b7999168)

To connect to my EC2 instance, I used a key pair (download into my local computer), and used they key pair's details to SSH into the EC2 instance from y local computer's terminal.  

To help me create my web app, I first updated all existing software on my EC2 instance using the command sudo dnf update -y. Following that crucial step, I installed the core software components needed for the web application to run. This involved installing httpd (the Apache web server) to serve web content; php which is the programming language the web app will use; php-mysqli, a PHP extension essential for enabling the application to connect to our MySQL-compatible Aurora database; and mariadb105, which provides important client libraries so the EC2 instance (our web server) can communicate effectively with the Aurora database. After successfully installing these packages, I then started the Apache web server using sudo systemctl start httpd to get the basic web service operational on the EC2 instance.

---

## Connecting my Web App to Aurora

I set up my EC2 instance's connection details to my database by first connecting to my EC2 instance via SSH and navigating to the /var/www directory, which is the web root. I encountered a permissions issue, so I had to use the sudo chown ec2-user:ec2-user /var/www command to change the ownership of the directory, allowing my ec2-user to create files and folders within it.

Once the permissions were corrected, I created a new sub-folder named inc and then, inside this inc folder, I created a new file named dbinfo.inc. I used the nano text editor to open and edit this dbinfo.inc file.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-webapp_1709b25b)

---

## My Web App Upgrade

Next, I upgraded my web app by navigating to the /var/www/html directory on my EC2 instance, which is the root directory for web files. There, I created a new file named SamplePage.php using the command >SamplePage.php and then opened it for editing with the nano text editor.

Into this SamplePage.php file, I pasted a comprehensive PHP script. This script was designed to make the web application interactive and database-driven.

After adding this code, I saved and closed the SamplePage.php file. This transformed the basic web page into a dynamic application capable of reading from and writing to the Aurora database.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-webapp_2709b25b)

---

## Testing my Web App

To make sure my web app was working correctly, I directly accessed and queried the Aurora database using the MySQL Command Line Interface (CLI) from within my EC2 instance.

First, since it wasn't already installed, I installed the MySQL client software on the EC2 instance. This involved downloading the MySQL repository and then running sudo yum install mysql-community-client -y.

Once the MySQL client was ready, I established a connection to my Aurora database by using the command mysql -h YOUR_ENDPOINT -P 3306 -u admin -p, making sure to use my specific Aurora writer instance endpoint and then entering my database password when prompted.

After successfully connecting to the database via the CLI, I selected my target database with USE sample;. The crucial step was then to run the SQL query SELECT * FROM EMPLOYEES;. This command displayed all the data currently stored in the EMPLOYEES table, allowing me to verify that the information entered.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-webapp_1409z22b)

---

---
