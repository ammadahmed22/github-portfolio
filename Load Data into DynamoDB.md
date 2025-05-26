<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Load Data into DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-dynamodb)

**Author:** Ammad Ahmed  
**Email:** ayaanahmedcoding@gmail.com

---

## Load Data into a DynamoDB Table

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_b481c730)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed, serverless, key-value NoSQL database service provided by AWS, designed for high-performance applications at any scale. It's useful because it offers significant flexibility in how data is structured; unlike relational databases, DynamoDB tables don't require a fixed schema. Each item in a table can have its own unique set of attributes, making it ideal for applications with diverse or evolving data types, such as e-commerce product catalogs or user profiles. Another key reason it's useful is its speed and scalability. DynamoDB uses partition keys to efficiently distribute data and quickly locate items, enabling low-latency data access even as data volumes grow. Users also define read and write capacity units (RCUs and WCUs) to control performance and costs, and it offers a generous free tier, making it accessible for various project sizes.

### How I used Amazon DynamoDB in this project

In today's project, I used Amazon DynamoDB to set up a non-relational database solution for NextWork to store data like projects, videos, and community activity. My first step was to create an initial DynamoDB table from scratch, understanding its basic structure of items and attributes, and learning about provisioned throughput with Read and Write Capacity Units. Following that, I used AWS CloudShell and the AWS Command Line Interface (CLI) to execute a script that created four specific DynamoDB tables: ContentCatalog, Forum, Post, and Comment. For each of these tables, I defined their primary key structures, including partition keys and, for some, sort keys, along with their initial throughput settings. After creating these tables, I then proceeded to load data into them and also practiced viewing and updating that data, which are essential data management operations.

### One thing I didn't expect in this project was...

One aspect of this project that might have been particularly eye-opening or less expected was the sheer degree of schema flexibility offered by DynamoDB and how fundamentally different its data organization is compared to a relational database. While the concept of a non-relational database was introduced, actually seeing that items within the same table could have completely different attributes, without needing predefined columns for every possible piece of data, could be quite a shift in perspective. The analogy of it being like a spreadsheet where "every row can have a different number of columns and different column headers" really drives home this flexibility. Understanding how this impacts data modeling and why it's so beneficial for certain use cases, like storing diverse product types, was likely a more profound realization than just learning another database type.

### This project took me...

This project took me about 1 hour to finish. 

---

## Create a DynamoDB table

DynamoDB tables organize data using a flexible, non-relational structure based on items and their attributes, rather than the strict rows and columns found in traditional relational databases. Think of a DynamoDB table as a collection of items, where each item (like a specific student, e.g., 'Nikko') is a distinct entity. Each of these items then has its own set of attributes, which are individual pieces of data describing that item (for example, 'ProjectsComplete' could be an attribute for Nikko).

The key difference and flexibility here is that each item within the same table can have a completely different set and number of attributes. It’s like having a spreadsheet where every row (item) can have its own unique column headers (attributes) and a varying number of columns, which is a level of schema flexibility not typically possible with relational databases.

An attribute is a fundamental piece of data that describes a characteristic or property of an item in Amazon DynamoDB. Think of it like a field or a column entry for a specific record. For instance, if you have an item representing a person named Nikko, an attribute could be 'ProjectsCompleted' with a value like '5'. Each item in DynamoDB can have multiple attributes, and importantly, unlike traditional relational databases, different items within the same table can have their own unique sets of attributes, offering great flexibility.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_a3cefee0)

---

## Read and Write Capacity

Read capacity units (RCUs) and write capacity units (WCUs) are the way Amazon DynamoDB measures and provides the throughput capacity for read and write operations on your tables; you can think of them as the number of 'engines' DynamoDB uses for these tasks. One RCU gives your table the ability for DynamoDB to perform a maximum of two read operations every second. In a similar way, WCUs provide the capacity for write operations such as editing, updating, or deleting data, where one WCU allows for one item write operation per second. The quantity of RCUs and WCUs you set up for your table dictates its performance capabilities and directly influences how AWS calculates charges for its use.

Amazon DynamoDB's Free Tier covers 25GB of data storage, 25 Write Capacity Units (WCUs), and 25 Read Capacity Units (RCUs) per month. This allocation is generally enough to handle up to 200 million requests monthly, all without incurring any charges. I turned off auto scaling because for this project, I want to have explicit control over the provisioned throughput. This helps ensure that the table's capacity remains within the Free Tier limits, preventing any unexpected scaling that could potentially lead to charges if it were to exceed those free allowances, especially in a learning or testing environment.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_ef47dd8f)

---

## Using CLI and CloudShell

AWS CloudShell is shell in your AWS Management Console, which means it's a space for you to run code! The awesome thing about AWS CloudShell is that it already has AWS CLI pre-installed.

What is CLI?
AWS CLI (Command Line Interface) is a software that lets you create, delete and update AWS resources with commands instead of clicking through your console.

You usually have to install AWS CLI into your computer to use it, but in our case, CloudShell already has CLI installed for us (thank you CloudShell ).

I ran a CLI command in AWS CloudShell that created four distinct Amazon DynamoDB tables, each with its own specific schema and initial settings. This involved executing a script containing multiple aws dynamodb create-table commands.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_81e0258b)

---

## Loading Data with CLI

I ran a CLI command in AWS CloudShell that load multiple pieces of data (i.e. load multiple items) into DynamoDB tables I set up in the previous step. This AWS CLI command is structured as 'aws dynamoDB batch- write-item -- request items file://.. '

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_791c600b)

---

## Observing Item Attributes

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_b481c731)

I checked a ContentCatalog item, which had the following attribute: Id, Authors, ContentType, Difficulty, Price, ProjectCategory, Published, Title, and URL.

I checked another ContentCatalog item, which had a different set of attributes: Id, ContentType, Price, Services, Title, URL, and VideoType.

---

## Benefits of DynamoDB

A benefit of DynamoDB over relational databases is flexibility, because DynamoDB allows each item within a table to have its own unique set of attributes, whereas relational databases require every row in a table to conform to a predefined schema with the same columns. This means in DynamoDB, you don't have to assign a value for an attribute to every single item if it doesn't apply, which is often necessary in relational systems. For instance, when storing data for an e-commerce site, different products might have vastly different characteristics (e.g., a book has an 'author' while a shirt has a 'size' and 'color'). DynamoDB can easily accommodate these varying attributes within the same table for different items without needing to create columns that would be empty for many other items. This makes it highly adaptable for evolving data structures or when items in a collection naturally have diverse properties.

Another benefit over relational databases is speed, because DynamoDB tables are architected to use partition keys to efficiently distribute and locate data, allowing for very fast retrieval. When you request an item using its partition key, DynamoDB can directly navigate to the specific partition where that data resides without needing to search through unrelated data. In contrast, the information provided suggests that relational databases sometimes have to scan through much larger portions of an entire table to find particular data, a process that can slow down performance, especially as the database grows in size. This ability of DynamoDB to use partition keys for targeted access is a key factor in its high-speed performance.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-dynamodb_b481c730)

---

---
