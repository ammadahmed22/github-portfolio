<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Peering

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-peering)

**Author:** Ammad Ahmed  
**Email:** ayaanahmedcoding@gmail.com

---

## VPC Peering

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_88727bef)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is your own private, logically isolated section of the AWS cloud where you can define and control a virtual network to launch your AWS resources. It's incredibly useful because it provides the foundational layer for security and network architecture in AWS. With a VPC, you can create a secure space for your applications, controlling inbound and outbound network traffic using tools like security groups and network ACLs. 

### How I used Amazon VPC in this project

In today's project, which focused on VPC Peering, I used Amazon VPC to create and connect two separate network environments. I started by launching two distinct VPCs, each with a unique IP CIDR block to prevent overlap. Each VPC was configured with its own public subnet, internet gateway, and route tables using the "VPC and more" wizard. The core of the project was then to create a VPC peering connection to act as a direct networking bridge between these two VPCs. To make this connection functional, I updated the route tables in each VPC to direct traffic destined for the peer VPC's CIDR block through the peering connection. 

### One thing I didn't expect in this project was...

One thing that was particularly insightful in this project was the realization that simply creating and accepting a VPC peering connection does not automatically enable communication between the two VPCs. I might have initially assumed that once the peering "bridge" was active, resources could immediately talk to each other. However, I learned that you must also explicitly update the route tables in each VPC to tell the network traffic how to find and use that new path. This extra configuration step—adding a route for the other VPC's CIDR block that targets the peering connection—was a crucial and perhaps unexpected requirement. It clearly demonstrated that VPC components are distinct and require deliberate configuration to work together, reinforcing how important routing is to any network's functionality.

### This project took me...

This project took approximately 1 hour to complete.

---

## In the first part of my project...

### Step 1 - Set up my VPC

In this step, I'm going to rapidly create the foundational network environments needed for the VPC peering project. Leveraging what I learned in the last project, I will be creating two brand new, separate Virtual Private Clouds (VPCs) from scratch. To do this efficiently, I'll be using the AWS VPC wizard's "VPC and more" feature, which utilizes the visual VPC resource map to set up all the necessary components like subnets and route tables very quickly. The purpose of this is to get both VPCs provisioned so that I can then proceed to the main task of connecting them together with a VPC peering connection.

### Step 2 - Create a Peering Connection

In this step, with my two VPCs ('NextWork-1' and 'NextWork-2') now created and ready, I'm going to establish the networking bridge between them by creating a VPC peering connection. This action sets up a direct, one-to-one connection link that will allow these two separate VPCs to communicate with each other privately. The purpose of this is to enable resources, like EC2 instances, in one VPC to "talk" to resources in the other VPC using their private IP addresses, just as if they were part of the same network. This is the foundational step for achieving inter-VPC connectivity.

### Step 3 - Update Route Tables

In this step, with the peering connection now active between my two VPCs, I need to actually instruct the traffic within each VPC on how to use this new bridge.To do this, I will be updating the route tables for the relevant subnets in both 'NextWork-1' VPC and 'NextWork-2' VPC.

Specifically, I'll add a new route to VPC 1's route table that directs any traffic destined for VPC 2's IP address range to go through the peering connection. Then, I'll do the same in reverse for VPC 2's route table, telling it to send traffic bound for VPC 1's address range across that same peering connection. This is a critical configuration, because just creating the peering connection isn't enough; these route table updates are what actually enable resources in each VPC to find and communicate with each other. 

### Step 4 - Launch EC2 Instances

In this step, I'm going to launch a new Amazon EC2 instance inside each of my two VPCs ('NextWork-1' and 'NextWork-2'). This means I'll have one active virtual server running in VPC 1 and another one running in VPC 2. The purpose of doing this is to place resources within each network that I can use to validate our setup. These two instances will act as the endpoints for my connectivity tests. Later, I'll use them to send traffic (like a ping) from one instance to the other to confirm that the VPC peering connection and the route tables I configured are working correctly, and that the two VPCs can indeed communicate with each other.

---

## Multi-VPC Architecture

I started my project by launching two separate Virtual Private Clouds (VPCs) from scratch using the "VPC and more" wizard for each. The first VPC was named 'NextWork-1' and configured with a single public subnet in one Availability Zone. The second VPC was named 'NextWork-2' and set up with an identical structure: one public subnet in one Availability Zone. The wizard also automatically provisioned other essential components for each VPC, such as an internet gateway and the necessary route tables to make the subnets public. In total, since each of the two VPCs has one subnet, I created two subnets in this step.

The CIDR blocks for VPCs 1 and 2 are 10.1.0.0/16 for 'NextWork-1' and 10.2.0.0/16 for 'NextWork-2'. They have to be unique because having overlapping IP address ranges between the two VPCs would cause routing conflicts and prevent resources from communicating correctly if the VPCs were ever connected to each other. Since the goal of this project is to eventually connect these two VPCs using VPC Peering, it's essential that their IP address spaces do not overlap. This ensures that every resource across both VPCs can have a unique IP address, allowing network traffic to be routed unambiguously between them once the peering connection is established.

### I also launched 2 EC2 instances

I didn't set up key pairs for these EC2 instances as this project will utilize EC2 Instance Connect for SSH access, and that service conveniently manages the key pair process automatically. In previous projects, I learned the manual method of creating, downloading, and using a .pem file for secure connections. However, for this project, relying on EC2 Instance Connect streamlines the setup. It works by generating a temporary, one-time-use key pair each time I connect from the AWS console, removing the need for me to manage the private key files myself. Since the primary focus here is on testing VPC peering connectivity and I've already practiced manual key pair management, this is a more efficient approach. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_11111111)

---

## VPC Peering

A VPC peering connection is a direct, private networking link established between two separate Virtual Private Clouds (VPCs). This connection allows resources, like EC2 instances, within the two different VPCs to communicate with each other directly using their private IP addresses, just as if they were on the same network. It effectively creates a private bridge between them, ensuring that the data transferred between the VPCs stays within the secure AWS network and does not have to traverse the public internet. This type of connection can be set up between VPCs in the same AWS account or even between VPCs in different AWS accounts. 

VPCs would use peering connections to establish a secure, private, and efficient communication path between resources that reside in separate VPCs. The primary purpose is to allow these distinct VPCs to share data and services without exposing that traffic to the public internet. This enhances security by keeping inter-VPC communication within the AWS network backbone. It can also improve performance and reduce data transfer costs compared to sending data over the internet. This capability is crucial for scenarios like businesses needing to securely collaborate by sharing resources across different AWS accounts, or for an organization to connect VPCs that might be segmented for different environments (e.g., development and production) or departments. 

The difference between a Requester and an Accepter in a peering connection is their role in the two-step process of establishing the connection. The Requester is the owner of the VPC that initiates the peering connection request; they are the one sending the "invitation to connect" to the other VPC. The Accepter, on the other hand, is the owner of the VPC that receives this invitation. The peering connection is not active until the Accepter explicitly accepts the request. This two-sided handshake mechanism ensures that peering connections are always consensual and that both parties must agree before a private network link is established between their VPCs. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_1cbb1b88)

---

## Updating route tables

After accepting a peering connection, my VPCs' route tables need to be updated because the peering connection itself only creates a potential pathway between the two VPCs; it doesn't automatically direct any traffic through it. By default, the resources in each VPC only know how to route traffic locally within their own network. To enable actual communication across the peering connection, I had to explicitly add a route to the route table of each VPC. This new route acts as a "road sign," telling the VPC how to send traffic destined for the other VPC's IP address range by directing it through the newly established peering connection. Without these routes, the VPCs would remain isolated from each other despite the active peering link.

My VPCs' new routes have a destination of the other VPC's IPv4 CIDR block. Specifically, in the route table for VPC 1 (which has a CIDR of 10.1.0.0/16), I added a route with a destination of 10.2.0.0/16 (VPC 2's CIDR). Symmetrically, in VPC 2's route table, a route with a destination of 10.1.0.0/16 (VPC 1's CIDR) would be needed. The routes' target was the specific VPC peering connection resource that I had created (e.g., an identifier like pcx-xxxxxxxxxx). This setup ensures that whenever a resource in one VPC tries to send traffic to an IP address within the other VPC's address range, the route table correctly directs that traffic over the private peering connection.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_4a9e8014)

---

## In the second part of my project...

### Step 5 - Use EC2 Instance Connect

In this step, I'm going to begin the process of testing my VPC peering setup by first trying to connect to my initial EC2 instance (the one in 'NextWork-1' VPC) using EC2 Instance Connect. My immediate goal is to get a secure shell connection to this first server. The information also hints that I might run into a connection error, so a crucial part of this step will be to diagnose and resolve any issues that prevent me from connecting successfully, which will likely involve adjusting security group rules. Once I have access to this first instance, I can then use it as a base to test connectivity to the second instance across the peering connection.

### Step 6 - Connect to EC2 Instance 1

In this step, with an Elastic IP now assigned to my first EC2 instance, I'm going to make a second attempt to connect to it using EC2 Instance Connect. The immediate goal is to see if having a public IP address resolves the initial connection failure. However, the instructions suggest I might encounter yet another, different error! So, a key part of this step will be to diagnose this new potential problem and then implement the correct fix, likely related to security group rules. This is all part of the process of securing a stable connection to my first server, which I need as a launchpad for testing the VPC peering connection itself. 

### Step 7 - Test VPC Peering

In this step, now that I have successfully connected to the EC2 instance in my first VPC, I'm going to conduct the actual test of the VPC peering connection itself. From the command-line terminal of Instance 1, I will attempt to send test messages (likely using the ping command) to the private IP address of Instance 2, which resides in the second VPC. The instructions suggest that this initial test might not work right away, so a key part of this step will be to troubleshoot and resolve any connection errors I encounter until Instance 2 is able to receive the test messages and successfully send responses back to Instance 1. The ultimate goal is to validate that my entire setup—the peering connection, the route tables, and the security groups—is correctly configured to allow private, two-way communication between the two VPCs.

---

## Troubleshooting Instance Connect

Next, I used EC2 Instance Connect to establish a secure, command-line SSH connection to my EC2 instance directly from the AWS Management Console. The purpose of doing this was to gain direct access to the server's operating system so I could perform network connectivity tests, such as running ping or curl commands from that instance to other resources. EC2 Instance Connect provides a convenient way to get this access without needing to manage my own SSH keys locally.

I was stopped from using EC2 Instance Connect as the EC2 instance I was trying to connect to did not have a public IPv4 address associated with it. EC2 Instance Connect requires a public IP address to function because it establishes the connection from the AWS console over the internet to the instance. Since my instance only had a private IP at the time, there was no public endpoint for the service to connect to, which prevented the connection from being established. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_7685490c)

---

## Elastic IP addresses

To resolve this error, I set up Elastic IP addresses. Elastic IP addresses are static, public IPv4 addresses that you can allocate to your AWS account and then associate with one of your resources, such as an EC2 instance. Unlike the default dynamic public IPs that can change whenever an instance is stopped and started, an Elastic IP address is persistent and remains associated with your account until you explicitly release it. This provides a permanent, unchanging public address for your instance, much like having a fixed street address for a building.

Associating an Elastic IP address resolved the error because it provided my EC2 instance with the public IPv4 address that it was previously lacking, which is a key prerequisite for EC2 Instance Connect to work. By allocating an Elastic IP from Amazon's pool and attaching it to my instance, the instance instantly gained a public endpoint that was reachable from the internet. This gave the EC2 Instance Connect service a target destination to connect to, allowing it to successfully establish the secure SSH connection that had previously failed due to the absence of a public IP.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_45663498)

---

## Troubleshooting ping issues

What was the command you ran to test VPC peering?
To test VPC peering, I ran the command ping <Private_IPv4_address_of_Instance_2> from the command-line terminal of my first EC2 instance (Instance 1). For example, if the private IP of the second instance was 10.2.0.100, the command would be ping 10.2.0.100. This command instructs Instance 1 in my first VPC to send ICMP echo request packets directly to the private IP address of Instance 2 in my second VPC, which is the essential test to see if they can communicate across the peering connection.

A successful ping test would validate my VPC peering connection because it demonstrates that two-way, private communication is correctly configured and functioning between the two separate VPCs. When the ping command from Instance 1 receives replies from Instance 2, it confirms that the ICMP data packets are able to successfully travel from the source subnet in VPC 1, through its route table, across the VPC peering connection, through VPC 2's route table to the target subnet, and finally be accepted by the target instance's security group. The successful reply journey back validates the same path in reverse, confirming the entire end-to-end private network path is working as intended.

I had to update my second EC2 instance's security group because its existing inbound rules did not permit incoming ICMP traffic, which is the protocol used by the ping command. The initial connection test failed because even though the Network ACLs and route tables were correct, the security group—acting as a final firewall at the instance level—was blocking the ping request packets. I added a new rule that set the 'Type' to 'All ICMP - IPv4' and, crucially, set the 'Source' to the specific CIDR block of my first VPC (10.1.0.0/16). This ensured that only ICMP traffic originating from within my trusted first VPC would be allowed to reach the second instance, resolving the error and allowing the ping test to succeed. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-peering_7a29d352)

---

---
