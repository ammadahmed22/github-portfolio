<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Visualize a Relational Database

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-rds)

**Author:** Ammad Ahmed  
**Email:** ayaanahmedcoding@gmail.com

---

## Visualise a Relational Database

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_1fddb0b5)

---

## Introducing Today's Project!

### What is Amazon RDS?

Amazon RDS, which stands for Relational Database Service, is a managed cloud database service offered by AWS. It simplifies the process of setting up, operating, and scaling a relational database within the AWS cloud environment. This service is particularly useful because it significantly reduces the operational burden on users. AWS takes responsibility for many time-consuming database administration tasks, such as hardware provisioning, initial database setup, applying patches, managing backups, and handling software updates. This allows users to avoid the complexities of managing their own physical servers and underlying infrastructure. Furthermore, RDS offers excellent scalability and flexibility, enabling users to easily adjust their database's compute resources or storage capacity as their needs change, all while adhering to a pay-as-you-go model. 

### How I used Amazon RDS in this project

In today's project, Amazon RDS was utilized in several key ways to build and secure a data visualization pipeline. Initially, I launched a new MySQL RDS instance, which functioned as the foundational relational database. To begin with, I configured this instance, including its associated security group, to permit connections from MySQL Workbench running on my local machine; this involved making it temporarily publicly accessible and whitelisting my IP address. Following this setup, I connected to the RDS instance using MySQL Workbench to perform essential database management tasks, such as creating a new schema, defining the necessary tables within that schema, and then populating these tables with data using SQL commands. A significant part of the project then involved implementing security best practices for this RDS instance. This included transitioning the RDS instance from being publicly accessible to a private configuration.

### One thing I didn't expect in this project was...

Reflecting on the project's progression and common learning experiences for those new to such cloud setups, one aspect that might have been unexpected was the depth of the security considerations, particularly concerning the initial public accessibility of the RDS instance. While making the database instance publicly accessible seemed like a straightforward way to connect with MySQL Workbench from a local machine early in the project, the realization of this being a "Big yikes!"—a significant security vulnerability—could have been a notable learning moment. The subsequent, detailed steps to make the RDS instance private and then carefully configure security group rules to strictly limit access to only QuickSight likely emphasized how critical secure network architecture is. This shift from a quick, open configuration to a properly secured, more complex one might have been a more involved but invaluable lesson than initially anticipated.

### This project took me...

The project took me around 1.5.

---

## In the first part of my project...

### Creating a Relational Database

I created my relational database by going to RDS in AWS and following the Easy Create steps. I set up the name and login details of my database.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_43343546)

---

## Understanding Relational Databases

A relational database is a structured way to organize information into tables, similar to spreadsheets, where each table consists of rows (representing individual records or instances of an entity) and columns (representing specific attributes of that entity). The "relational" part signifies that these different tables can be logically linked together based on relationships in the data they contain, often using primary and foreign keys, allowing for efficient data storage, retrieval, and management, typically through the use of SQL (Structured Query Language).

### MySQL vs SQL

The difference between MySQL and SQL is that SQL (Structured Query Language) is the standard programming language itself, used to communicate with and manage relational databases. In contrast, MySQL is a specific brand of relational database management system (RDBMS)—which is a type of software—that uses SQL as the language to perform operations like creating databases, storing data, and retrieving information.

Think of it this way: SQL is like the grammar and vocabulary of a language spoken to manage data, while MySQL is one of many distinct software programs (like a specific "librarian" system) that understands and uses that SQL language to organize and interact with the actual "library" of data.

---

## Populating my RDS instance

The first thing I did was make my RDS instance public because I need to connect to it from MySQL WorkBench.

I had to update the default security group for my RDS instance because this security group acts as a virtual firewall, controlling all traffic that can go into and out of my database. By default, it wouldn't permit connections from external applications like MySQL Workbench running on my local computer.

To enable MySQL Workbench to connect and allow me to manage the database (like creating tables and adding data), I needed to modify its inbound traffic rules. Specifically, I added a new rule that permits traffic exclusively from my current IP address. This change is crucial because it opens a secure communication channel for MySQL Workbench while ensuring that only my machine can access the database, which is an important security practice.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_91b9fd1g)

---

## Using MySQL Workbench

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_1fddb0b5)

To populate my database I used SQL in the MySQL Workbench app to create and populate my database tables. First I had to connect my RDS instance to MySQL using the Endpoint, port, username and password.

---

## Connecting QuickSight and RDS

o connect my RDS instance to QuickSight I made my security group around my RDS instance allow traffic from any IP address so that QuickSight can connect easily.

This solution is risky because my RDS database instance is currently configured to be publicly available on the internet. This is a significant security concern because it means that instead of being accessible only by authorized services like QuickSight or my specific IP address for MySQL Workbench, it's potentially exposed to anyone. This public exposure makes the database highly vulnerable to unauthorized access attempts from hackers, bots, and malicious individuals who could try to steal, corrupt, or disrupt the data and the database service itself.

### A better strategy

First, I made a new security group so that my QuickSight will be secure.

Next, I connected my new security group to QuickSight by creating a connection to QuickSight and my VPC. and then my security group. I had to update my IAM role that was used to do this.

---

## Now to secure my RDS instance

To make my RDS instance secure I made it not publicy accessible and then created a new security group for my RDS instance.

I made sure that RDS instance could be accessed from QuickSight by creating the correct inbound rules that allowed querying of my RDS instance from my QuickSight security group.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_1709b26b)

---

## Adding RDS as a data source for QuickSight

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_1709b29b)

This data source is different from my initial data source because it is now significantly more secure! Previously, to establish a connection, my RDS instance might have been publicly accessible or using broader default security settings. Now, the RDS instance itself is configured to be private (not publicly accessible from the internet). Access is tightly controlled through a specific security group setup where my RDS instance's security group is configured to only accept connections from the dedicated security group associated with QuickSight. This is a much more robust and secure way to manage access compared to relying on default configurations or general public accessibility.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-rds_1709b30b)

---

---
