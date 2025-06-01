<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Creating a Private Subnet

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-private)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

## Creating a Private Subnet

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-private_afe1fdbd)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is your own logically isolated, private section within the AWS cloud, where you can define and control a virtual network to launch your AWS resources. It's incredibly useful because it gives you foundational control over your cloud networking environment, similar to managing your own traditional data center network but with the scalability and flexibility of the cloud. With a VPC, you can specify your own IP address ranges, create subnets for segmenting resources, configure route tables to direct traffic flow, and set up internet gateways for internet connectivity. This enables you to build a secure and organized network architecture, isolate resources for enhanced security (like placing databases in private subnets), control inbound and outbound traffic at both the subnet (with Network ACLs) and resource (with Security Groups) levels, and ultimately design a network that precisely fits your application's security and operational requirements.

### How I used Amazon VPC in this project

In today's project, which has been an extensive journey through AWS networking, I used Amazon VPC to design and construct a secure and functional network infrastructure from the ground up. 

### One thing I didn't expect in this project was...

One thing that was particularly enlightening and perhaps less expected in this project was the distinct default behaviors of different AWS security components, especially the contrast between default Network ACLs and custom Network ACLs.

### This project took me...

Not that long about 45 min 

---

## Private vs Public Subnets

Okay, let's dive into private and public subnets!

What is the difference between private and public subnets?
The difference between public and private subnets is that public subnets have a direct route to an internet gateway, which allows resources within them (like web servers with public IP addresses) to communicate directly with the internet for both inbound and outbound traffic. This makes them suitable for hosting internet-facing applications. In contrast, private subnets do not have a direct route to an internet gateway, meaning resources within them cannot be directly accessed from the internet, nor can they initiate outbound connections to the internet without additional configurations like a NAT gateway. They are designed to host backend resources that require isolation from public exposure.

Having private subnets are useful because they significantly enhance the security and control over your backend resources within a VPC. By isolating resources like databases, internal application servers, or sensitive data processing workloads from direct internet access, private subnets reduce their exposure to external threats and unauthorized access attempts. This layered security approach allows these internal resources to communicate securely with other resources within the VPC (for example, with web servers in a public subnet) and, if needed, to access the internet in a controlled manner (e.g., for software updates via a NAT gateway located in a public subnet) without being directly reachable from the outside world. This helps in building more secure and resilient application architectures.

My private and public subnets cannot have the same overlapping IP address CIDR blocks within the same VPC. Each subnet, whether it's designated as public or private, must be allocated a unique and distinct range of IP addresses from the VPC's overall CIDR block. This ensures that there are no IP address conflicts within the VPC and that network traffic can be correctly routed to the appropriate subnet based on the destination IP address. If subnets had overlapping IP ranges, it would create ambiguity and routing problems for the network traffic.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-private_afe1fdbd)

---

## A dedicated route table

By default, my private subnet is associated with the main route table of the VPC it was created in. When you create a new subnet and don't explicitly assign a different route table to it, AWS automatically associates it with the VPC's main route table. This main route table initially contains a local route that allows traffic to flow between all subnets within that VPC.

I had to set up a new route table because I want to define specific and distinct routing rules for my private subnet that differ from those applied to public subnets or the VPC's main route table, especially to ensure its privacy. For instance, the main route table might be configured with a route to an internet gateway to allow public internet access for public subnets. By creating a dedicated route table for my private subnet and ensuring it does not have a route to an internet gateway, I can explicitly control its network traffic flow, keeping it isolated from direct internet access and tailoring its connectivity (e.g., only allowing local VPC traffic or later adding a route to a NAT gateway for controlled outbound internet access). This helps maintain a more secure and organized network architecture.

My private subnet's dedicated route table only has one inbound and one outbound rule that allows traffic destined for IP addresses within the VPC's own CIDR block to be routed locally within the VPC. This is typically represented by a single route where the destination is the VPC's CIDR range (e.g., 10.0.0.0/16) and the target is local. This means resources in the private subnet can communicate freely with other resources in any subnet within the same VPC. Crucially, this dedicated route table for a private subnet does not contain a route to an internet gateway (e.g., a route for 0.0.0.0/0 to an IGW), which is what prevents direct outbound internet access from this subnet and helps maintain its private nature.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-private_b4b904b5)

---

## A new network ACL

By default, my private subnet is associated with the main, or default, network ACL (NACL) of the VPC it resides in. When a VPC is created, AWS automatically generates a default NACL for it. If you then create a new subnet within that VPC and don't explicitly associate a different, custom NACL with that subnet, it will automatically use this VPC's default NACL. This default NACL is initially configured to allow all inbound and all outbound traffic, which is why, for enhanced security of a private subnet, we typically create and associate a new, more restrictive custom NACL. 

I set up a dedicated network ACL for my private subnet because the default network ACL that it was initially associated with allows all inbound and all outbound traffic by default. While my private subnet's route table already prevents direct internet access, relying solely on that isn't ideal for robust security. A permissive default ACL could still expose resources within the private subnet to potential threats, for example, if another part of my VPC (like a public subnet) were compromised. By creating a new, custom network ACL for my private subnet, I can implement much stricter, more granular traffic filtering rules—starting with denying all traffic—to significantly enhance its security posture and ensure only explicitly authorized communication occurs. This adds an important layer of defense. 

My new network ACL has two simple rules - or more precisely, two default behaviors governing its inbound and outbound traffic, as custom NACLs operate on a "deny by default" principle. This means that immediately after creation and before any explicit rules are added, my new "NextWork Private NACL" automatically denies all inbound traffic trying to enter the associated private subnet, and it also automatically denies all outbound traffic attempting to leave the private subnet. It effectively starts with a clean slate where nothing is permitted, and I will need to add specific "allow" rules later to define exactly what kind of traffic should be allowed in or out based on the subnet's requirements. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-private_1ed2cb07)

---

---
