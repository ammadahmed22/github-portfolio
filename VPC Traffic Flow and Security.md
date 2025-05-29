<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Traffic Flow and Security

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-security)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

## VPC Traffic Flow and Security

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-security_92b0b0b4)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is your own logically isolated, private section within the AWS cloud, where you can launch AWS resources in a virtual network that you define and control. It's incredibly useful because it provides the foundational layer for security and network architecture in AWS. With a VPC, you can create a private space for your resources, controlling inbound and outbound network traffic using tools like security groups and network ACLs. This allows you to define your own IP address ranges, create subnets for organizing resources, configure route tables to manage traffic flow, and set up internet gateways for connectivity. Essentially, VPCs enable you to build a secure, scalable, and highly configurable network environment, much like your own traditional data center, but with the flexibility and scalability of the cloud.

### How I used Amazon VPC in this project

In today's project, which focused on the fundamentals of AWS networking, I used Amazon VPC to learn how to build and configure the core components of a private cloud network from scratch. This involved understanding why VPCs are important and then creating a custom VPC. I defined its IPv4 CIDR block and then created subnets within it, learning about the distinction between public and private subnets and how to enable auto-assignment of public IP addresses. I also worked with internet gateways, attaching one to my VPC, and then configured route tables to direct traffic, including creating a default route to the internet gateway to make a subnet public. Furthermore, I explored network security by setting up security groups with specific inbound and outbound rules to control traffic at the resource level, and learned about Network ACLs for subnet-level traffic control.

### One thing I didn't expect in this project was...

One aspect of this project that was particularly enlightening, and perhaps less expected, was the level of detailed configuration and the number of interconnected components required to make even a simple part of a VPC, like a public subnet, actually functional and truly "public." For instance, just creating a subnet and labeling it "public" isn't enough. I learned that it also needs an internet gateway attached to the VPC, a specific route in its associated route table pointing 0.0.0.0/0 traffic to that internet gateway, and appropriate security group (and potentially Network ACL) rules. The realization that several distinct networking resources must be correctly configured and linked together to achieve what seems like a basic outcome—internet connectivity for a subnet—was a key takeaway. It really highlighted the importance of understanding each piece and how they fit together in the larger networking puzzle. 

### This project took me...

This took me 1 hour to complete. 

---

## Route tables

Route tables are sets of rules, called routes, that act like a GPS or a traffic controller for your Virtual Private Cloud (VPC). They determine where network traffic originating from your subnets is directed. Every subnet within your VPC must be associated with a route table, as this table contains the necessary instructions—the routes—that tell your subnet's traffic how to travel to send and receive data, whether that's to other resources within the VPC, to the internet, or to other connected networks.

Route tables are needed to make a subnet public because they contain the specific instructions that allow traffic from that subnet to reach the internet. For a subnet to be considered public, its associated route table must have a route that explicitly directs internet-bound traffic (typically traffic destined for 0.0.0.0/0, which means all IPv4 addresses not otherwise specified) to an internet gateway (IGW). Without this route to an IGW, even if a subnet has resources with public IP addresses, the traffic from those resources has no path to leave the VPC and access the internet, nor can external traffic reach them directly through the IGW. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-security_0a07b191)

---

## Route destination and target

Routes are defined by their destination and target, which mean the 'destination' specifies the range of IP addresses where the network traffic is intended to go, and the 'target' indicates the gateway, network interface, or connection through which that traffic should be sent to reach its specified destination. For example, a destination of 10.0.0.0/16 might have a target of local, meaning traffic for that IP range stays within the VPC. Conversely, a destination of 0.0.0.0/0 (representing all IP addresses) might have a target like igw-xxxxxx, indicating that traffic not destined for the local VPC should be routed to the internet via that specific internet gateway. 

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0 and a target of the internet gateway (e.g., an identifier like igw-xxxxxx). The destination 0.0.0.0/0 is a special value that represents all possible IPv4 addresses, effectively acting as a default route for any traffic that doesn't match more specific routes within the route table. The target being the internet gateway means that this default traffic—anything not headed for another resource inside my VPC—is sent out to the public internet through that gateway. This is the standard configuration for enabling internet access for a public subnet.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-security_0a07b191)

---

## Security groups

Security groups are like virtual firewalls or security guards that operate at the resource level, such as for an EC2 instance, within your VPC. They don't attach to subnets or VPCs directly, but rather to specific resources like individual instances. Every resource that requires network traffic control needs to be associated with at least one security group. Their primary responsibility is to control both incoming (inbound) and outgoing (outbound) traffic by enforcing a set of rules that you define. These rules specify what kind of traffic is allowed or denied based on factors like the protocol (e.g., HTTP, SSH), port number (e.g., 80 for HTTP), and source or destination IP addresses.

### Inbound vs Outbound rules

Inbound rules are the set of permissions that control what kind of network traffic is allowed to enter the resources associated with a particular security group. These rules are crucial for determining who or what can access your resources; for example, they are essential for allowing visitors to reach a public website hosted on an EC2 instance. I configured an inbound rule that allows HTTP traffic on port 80 from any IP address (source 0.0.0.0/0). While AWS flags this as potentially risky due to its openness, this specific configuration is typical and necessary for a public web server, as it ensures that anyone on the internet can access the website. 

Outbound rules are the set of permissions that control what kind of network traffic your resources (e.g., an EC2 instance) associated with a security group are allowed to send out to other locations, whether within your VPC or to the internet. These rules help manage how your server interacts with other services or parts of the internet, for instance, when your server needs to fetch data from an external API or send an email notification. By default, my security group's outbound rule allows all outbound traffic to any destination (0.0.0.0/0). This means that unless I specifically restrict it, my resources can initiate connections and send data to any IP address on the internet or within my VPC. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-security_92b0b0b4)

---

## Network ACLs

Network ACLs are Network Access Control Lists that function like a firewall or traffic cops for an entire subnet within your VPC. They operate at the subnet level, acting as a gatekeeper for all data packets attempting to enter or leave any resource within that subnet. Network ACLs inspect these data packets against a numbered list of rules you define, checking criteria like source/destination IP, port, and protocol to determine whether to allow or deny the traffic. They are a crucial tool for enforcing broad security policies across segments of your network. 

### Security groups vs. network ACLs

The difference between a security group and a network ACL is that network ACLs operate at the subnet level, applying broad traffic filtering rules to all resources within that subnet, while security groups operate at the individual resource level (like an EC2 instance), providing more granular, stateful firewall capabilities. Network ACLs are stateless, meaning you need to define separate rules for inbound and outbound traffic (including return traffic). Security groups, on the other hand, are stateful; if you allow an incoming request, the outgoing response is automatically permitted regardless of outbound rules. Using both provides a layered security approach: NACLs for broad subnet-level filtering and security groups for fine-grained control at the resource instance. 

---

## Default vs Custom Network ACLs

### Similar to security groups, network ACLs use inbound and outbound rules

By default, a network ACL's inbound and outbound rules will allow all traffic to flow both into and out of the associated subnets. When AWS creates a default VPC, it also creates a default network ACL for it. This default ACL is configured with rules (typically rule number 100 for both inbound and outbound) that explicitly permit all types of traffic from all sources to all destinations. This permissive setup ensures that resources can communicate freely until you decide to implement more specific, restrictive rules.

In contrast, a custom ACL’s inbound and outbound rules are automatically set to deny all traffic by default, both for inbound and outbound directions. When you create a new, custom network ACL, it starts with no "allow" rules. Instead, it only has a final, implicit deny rule (often represented by an asterisk rule that cannot be modified) which blocks any traffic that doesn't match an explicit "allow" rule you add. Therefore, with a custom NACL, you must explicitly create "allow" rules to permit any specific type of traffic you want to flow into or out of the subnets associated with it. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-security_4faeb056)

---

## Tracking VPC Resources

I created additional Virtual Private Cloud (VPC) resources, specifically a new VPC itself, an internet gateway, and a security group. Instead of my usual region, I used a different AWS region for these new resources, by specifying the appropriate region code in my AWS CLI commands, to simulate a multi-region deployment.

Teams would use multiple regions to enhance the performance, availability, and resilience of their applications for a global user base. Deploying resources in regions geographically closer to their end-users helps to reduce latency, making applications feel faster and more responsive. Furthermore, distributing infrastructure across multiple regions provides crucial disaster recovery capabilities and protects against regional outages; if one region encounters an issue (like a natural disaster or system failure), services can often be maintained or failed over to another operational region, ensuring business continuity.

EC2 Global View is a tool where you can find a consolidated dashboard showing your Amazon EC2 instances and related Virtual Private Cloud (VPC) resources, like VPCs and security groups, from across all AWS Regions in your account. It gives you a single, unified perspective of these resources, which is particularly helpful when managing multi-region deployments.

I could even narrow down my search by filtering the display to show resources within a specific AWS Region, allowing me to focus on one geographic area at a time if needed, rather than viewing everything at once.

Without EC2 Global View, you'd have to manually switch between each individual AWS Region within the Management Console to locate, track, and manage your resources. This would be a significantly more time-consuming and cumbersome process, especially if your infrastructure is spread across numerous regions globally, making it harder to get a quick overview of your entire AWS footprint. 

Now that I've learnt about EC2 Global View, I'd use it again to quickly get a comprehensive, bird's-eye view of all my EC2 instances, VPCs, subnets, and security groups spread across every AWS Region my account is active in, all from a single dashboard. This would be particularly helpful for tasks like:

Performing a complete resource inventory to understand my entire AWS networking footprint without having to manually switch between regions.
Managing and monitoring applications that are intentionally deployed across multiple regions for high availability, disaster recovery, or to serve a global user base with lower latency.
Identifying any forgotten or unused resources in less-frequently accessed regions, which can help with security audits and cost optimization.
Troubleshooting widespread issues by quickly checking resource status across different geographic locations.
Essentially, any time I need a holistic understanding of my EC2 and VPC resources beyond a single region, EC2.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-security_b03ea6162)

---

---
