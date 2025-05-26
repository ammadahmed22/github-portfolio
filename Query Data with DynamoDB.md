<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Query Data with DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-query)

**Author:** Ammad Ahmed  
**Email:** ayaanahmedcoding@gmail.com

---

## Query Data with DynamoDB

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-query_733d9399)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed, serverless, key-value NoSQL database service from AWS, known for its speed, scalability, and flexibility. It organizes data into tables, which are collections of items, and each item is a set of attributes. A key reason DynamoDB is useful is its schema flexibility: unlike relational databases, items in the same DynamoDB table do not need to have the same attributes or structure, making it ideal for evolving applications or diverse datasets like product catalogs or user profiles. It's also useful for its high performance at any scale, achieved through features like partition keys for efficient data distribution and retrieval, and provisioned throughput control via Read and Write Capacity Units (RCUs and WCUs). This allows for consistent, low-latency data access, and it offers a generous free tier, making it accessible for many use cases. Furthermore, it supports features like transactions for maintaining data consistency across multiple operations.

### How I used Amazon DynamoDB in this project

In today's project, I used Amazon DynamoDB to design and implement a non-relational database solution for NextWork to store various types of data such as content, forum discussions, posts, and comments. This involved several key activities: first, I created DynamoDB tables, initially one and then a set of four (ContentCatalog, Forum, Post, Comment), using both the AWS Management Console and more efficiently through AWS CloudShell with AWS CLI commands. For these tables, I defined their primary key structures, including partition keys (like Id or Name) and sort keys (like Subject or CommentDateTime), and set their initial provisioned throughput. After table creation, I loaded data into them and practiced viewing and updating this data. I then learned to query these tables, first using the console to find data with partition and sort keys, and later using AWS CLI get-item commands with options like projection expressions and consistent reads.

### One thing I didn't expect in this project was...

One particularly insightful aspect of this project that might have been less expected was how strictly DynamoDB enforces the use of its primary key, especially the partition key, for efficient querying, and the immediate impact if you don't adhere to this. The learning module highlighted an error scenario that occurs if you try to query items without specifying the partition key. While I understood that keys are important, experiencing or learning about this direct error and the subsequent explanation of why "data modeling is sooo important" really emphasized that you can't just query DynamoDB any way you like, as you might attempt with SQL in some relational databases. This underscored that designing your tables and access patterns around the primary key is not just a best practice but a fundamental requirement for effective and performant use of DynamoDB for many query types, which was a very practical and impactful lesson.

### This project took me...

The project took me about 1 hour this was due to running into an error I was previously working on my macbook for the previous project but when switching over to my Window PC when using cloudshell something happen and I kept getting errors so I had to delete my shell and start over again.

---

## Querying DynamoDB Tables

A partition key is the primary component of a table's primary key in Amazon DynamoDB and acts as a fundamental filter that DynamoDB uses to distribute data across different storage partitions and then efficiently locate items. 

A sort key is an optional, secondary component of a table's primary key in Amazon DynamoDB that works in tandem with a partition key to further organize and refine query results.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-query_d105b0b0)

---

## Limits of Using DynamoDB

I ran into an error when I queried for items in the DynamoDB table without specifically using the Id attribute in my query conditions. This was because Amazon DynamoDB requires the partition key—which in this case is Id—to be included when performing targeted query operations. My attempt to find data did not leverage this Id (partition key), and as a result, DynamoDB couldn't efficiently process the request and flagged it as an error. This experience underscores the importance of data modeling in DynamoDB; you must design your tables and queries with a clear understanding of your partition (and sort) keys, as failing to use them correctly can make retrieving specific data difficult or, as in this scenario, even impossible with standard query methods.

Insights we could extract from our Comment table includes easily retrieving all comments associated with a specific item Id (like all comments for a particular article or product), as this Id is our partition key. We can also get these comments sorted chronologically because CommentDateTime is our sort key. This means we can efficiently fetch comments for a specific item within a certain date/time range, find the most recent comments for that item, or count the total number of comments for that particular Id. These queries are efficient because they directly leverage the table's primary key structure.

Insights we can't easily extract from the Comment table includes finding all comments made by a particular user across all different Ids, assuming the user information isn't part of the primary key or an existing secondary index. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-query_cb3e260c)

---

## Running Queries with CLI

A query I ran in CloudShell was an aws dynamodb get-item command targeting the ContentCatalog table. This query will specifically attempt to retrieve the item that has a numeric Id of 202. If an item with this Id is found, the query is set up using a projection expression to return only three of its attributes: Title, ContentType, and Services, instead of all the item's data. Additionally, this command will also return information about the total read capacity units consumed by this specific get-item request. Since the --consistent-read flag wasn't used in this particular command, it will perform an eventually consistent read, which is the default behavior.

Query options I could add to my query are --consistent-read, --projection-expression, and --return-consumed-capacity, and they each modify how DynamoDB processes the request or what it returns.

The --consistent-read option is used when I need a guarantee that the data I'm reading is the absolute most recent version of that item, reflecting all successful writes that occurred before the read. This is important for applications where having the very latest data is critical, though it consumes more read capacity units than the default (eventual consistency).
The --projection-expression allows me to specify a list of attributes that I want DynamoDB to return. So, instead of getting the entire item with all its attributes, I can select only the specific pieces of information I need, which can be more efficient.
The --return-consumed-capacity option, when included, tells DynamoDB to include in the response how many read or write capacity units were used up by that particular operation. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-query_733d9399)

---

## Transactions

A transaction is a sequence of one or more database operations that are bundled together and treated as a single, atomic unit of work. The defining characteristic of a transaction is that either all the operations within it succeed and their changes are permanently committed to the database, or if any single operation in the group fails for any reason, then all changes made by the transaction are rolled back, and the database is left in its state prior to the transaction. This "all-or-nothing" approach is crucial because it ensures that any change involving multiple steps or multiple tables remains consistent across your database, thereby maintaining data integrit

I ran a transaction using the AWS Command Line Interface (CLI), because DynamoDB transactions involving multiple items, like this one, are best managed programmatically or via the CLI rather than the AWS Management Console. This transaction did two things simultaneously to keep the data consistent across my tables: first, it added a new item to the Comment table, which included all the details of a new comment submitted by 'User Connor'. Second, it updated the item in the Forum table corresponding to the 'Events' forum—where Connor's comment was made—by incrementing that forum's total comments count by one. Performing these two updates within a single transaction guaranteed that both the comment was logged and the forum's statistics were correctly updated together.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-databases-query_2f65f83e)

---

---
