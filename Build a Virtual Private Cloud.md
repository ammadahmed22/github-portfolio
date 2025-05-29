<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Virtual Private Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-vpc)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

## Build a Virtual Private Cloud (VPC)

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-vpc_2facf927)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon Virtual Private Cloud (VPC) is your own isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define.

It's useful because it gives you complete control over your virtual networking environment. You can select your own IP address ranges, create subnets, configure route tables, and set up network gateways – just like you would in your own data center. This provides security by allowing you to control inbound and outbound traffic, and flexibility in designing your network architecture.

### How I used Amazon VPC in this project

In today's project, you used Amazon VPC by:

Creating a new VPC: You initiated a VPC with a specific CIDR block (10.0.0.0/24) using the aws ec2 create-vpc command.
Tagging the VPC: You assigned a name ("Nextwork VPC 2") to your VPC for easier identification using aws ec2 create-tags.
Creating a subnet within the VPC: You then created a subnet (10.0.0.0/25) associated with your new VPC ID using aws ec2 create-subnet.
These steps established the foundational network structure for launching resources.

### One thing I didn't expect in this project was...

One thing you might not have expected was how the AWS CLI handles output for some commands. For instance, after creating the subnet, the detailed JSON output was likely displayed in a pager (like less). This required you to press q to exit before you could run subsequent commands, which led to the "Log file already in use" observation when you tried to create an internet gateway immediately after.

### This project took me...

This took me about 1 hour to complete. 

---

## Virtual Private Clouds (VPCs)

VPCs are Virtual Private Clouds, which essentially function as your own private, logically isolated section within the vast AWS cloud. They are crucial because they provide the ability to make your AWS resources private and give you direct control over your virtual network environment. Think of it this way: without VPCs, every AWS resource created by any user would exist in one giant, open, shared space, making it extremely difficult to manage security, privacy, and organization—like a country with no private properties or distinct districts, where everyone could see and access everyone else's assets. VPCs solve this by allowing you to define your own network, control how your resources communicate with each other (often without needing to go over the public internet), and ensure they are not just randomly scattered and exposed, but securely contained within a space you manage.

There was already a default VPC in my account ever since my AWS account was created. This is because AWS automatically sets up a default VPC for every new account to provide an immediate, usable private network space. This is incredibly helpful, especially for users who are new to AWS, because it means you can start launching resources that require a network, like EC2 instances, and begin connecting services together right from day one without first having to learn the intricacies of VPC creation and configuration. Essentially, the default VPC acts as a handy and functional starting point, ensuring that you can quickly begin using many AWS services that depend on a VPC to operate securely and effectively.


To set up my VPC, I had to define an IPv4 CIDR block, which is a method for specifying a range of IP addresses that will be available for use within my virtual private cloud. It's written as an IP address followed by a slash and a number, like 10.0.0.0/16. This number after the slash, called the prefix length, indicates how many bits of the IP address are fixed for the network portion, while the remaining bits can be allocated to individual resources within the VPC. For example, a /16 CIDR block like 10.0.0.0/16 means the first 16 bits (the 10.0 part) are constant, leaving 16 bits for host addresses, which allows for a large pool of 65,536 possible IP addresses (from 10.0.0.0 to 10.0.255.255). Essentially, defining this CIDR block is like establishing the overall address space or "zone" for my private network in AWS.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-vpc_2facf927)

---

## Subnets

Subnets are logical subdivisions of a Virtual Private Cloud (VPC), similar to distinct neighborhoods within a city, designed to help organize and isolate resources based on security and operational needs. You use them to group resources that might have similar access rules or restrictions, allowing for finer-grained network control. For example, you can create public subnets for resources that need direct internet access and private subnets for backend resources that should be shielded. Each subnet resides entirely within one Availability Zone and must have a unique IP address range (CIDR block) that doesn't overlap with other subnets in the same VPC.

There are already subnets existing in my account, one for every Availability Zone within the AWS Region where my default VPC is located. This is because when an AWS account is created, AWS automatically sets up a default VPC complete with these predefined subnets.

Once I created my subnet, I enabled the 'auto-assign public IPv4 address' setting for it. This setting makes sure that any Amazon EC2 instance launched within this specific subnet will automatically be allocated a public IPv4 address at the time of its creation. so that these instances can readily access the internet or be made accessible from the internet without requiring me to manually assign a public IP address to each individual instance. This is a huge time-saver and simplifies the process of deploying resources that need to communicate outside the VPC.

The difference between public and private subnets are that public subnets are configured to have a direct route to the internet, allowing resources within them (like web servers) to communicate with external networks, whereas private subnets are isolated from direct internet access and are intended for internal resources like databases that shouldn't be publicly exposed. For a subnet to be considered public, it has to not just be labeled as such, but critically, it must have a route in its associated route table that directs internet-bound traffic to an internet gateway. So, even if my subnet is named 'Public 1', it isn't actually a public subnet yet because this essential connection and route to an internet gateway—the component that enables direct internet connectivity—has not yet been set up.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-vpc_157c4219)

---

## Internet gateways

Internet gateways are key to making applications available on the internet. By attaching an internet gateway, your instances can access the internet and be accessible to external users.

Attaching an internet gateway to a VPC means that the resources within that VPC, such as EC2 instances, gain the ability to communicate with the internet. Specifically, it allows instances with public IP addresses to initiate outbound connections to the internet (e.g., to download software updates) and also makes those instances reachable from the internet, so any applications they host, like a website, become publicly accessible to users.

If I missed this step, then despite any other configurations like assigning public IP addresses to my EC2 instances or placing them in a subnet labeled as 'public,' those resources would remain isolated from the internet. My web applications wouldn't be reachable by external users, and the instances themselves wouldn't be able to access any external online services, effectively keeping my VPC private and disconnected from the global internet.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-vpc_4ae90410)

---

## Using the AWS CLI

VPC resources could also be created with CloudShell, which is a browser-based shell environment that you can access directly from the AWS Management Console, providing a convenient space to run code and commands. One of its great features is that it comes with the AWS Command Line Interface (CLI) pre-installed and already configured with your account credentials, saving you setup time.

CLI is the AWS Command Line Interface, a powerful software tool that enables you to interact with and manage your various AWS services and resources by typing commands in a terminal or command-line prompt, as an alternative to clicking through the graphical interface of the AWS console. Engineers often prefer the CLI because it allows for scripting and automation of tasks, making it an essential and efficient way to manage cloud environments, especially for complex operations requiring speed and versatility.

To set up a VPC or a subnet, you can use the command line interface, specifically AWS CLI commands like aws ec2 create-vpc for creating a VPC or aws ec2 create-subnet for adding a subnet. Make sure to avoid errors by including all the required parameters for the command you are running. For example, I ran into an error when trying to execute the create-subnet command because I initially omitted the mandatory --cidr-block parameter. The AWS CLI needs this CIDR block to define the IP address range for the new subnet; without it, the CLI doesn't have enough information to know which IP addresses to allocate, and thus the command fails. So, it's essential to carefully check and provide all necessary parameters like --cidr-block when creating network resources.

Compared to using the AWS Console, an advantage of using commands is automation and repeatability. You can script the entire VPC setup, ensuring consistency across environments and allowing for quick redeployment. Commands also facilitate version control of your infrastructure (Infrastructure as Code).

An advantage of using the Console is its visual interface and discoverability. It's often easier for beginners to understand the relationships between resources and to explore available options without needing to know specific command syntax. The Console also provides wizards and helpful tips that can simplify the creation process.

Overall, I preferred using commands for this task. While the Console is great for visualization and learning, the CLI's power for scripting, precision, and repeatability becomes invaluable for managing infrastructure efficiently, especially as complexity grows or when you need to replicate environments. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-vpc_9b2465411)

---

---
