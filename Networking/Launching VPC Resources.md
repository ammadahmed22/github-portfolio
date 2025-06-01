<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launching VPC Resources

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-ec2)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

## Launching VPC Resources

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-ec2_8ee57662)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is your own logically isolated, private section within the AWS cloud where you can launch and manage a virtual network that you define and control. 

### How I used Amazon VPC in this project

In today's project, which has spanned various aspects of AWS networking, I used Amazon VPC to learn and implement a comprehensive virtual network from the ground up.

### One thing I didn't expect in this project was...

One thing that was particularly insightful and perhaps less expected in this project was how effectively the "VPC and more" creation wizard, with its integrated VPC resource map, could simplify and clarify the setup of a best-practice VPC architecture. 

### This project took me...

About 30 mins having the VPC resoures from the previous project setup helped alot. 

---

## Setting Up Direct VM Access

Directly accessing a virtual machine means establishing a remote connection to the EC2 instance's operating system, allowing you to interact with it as if it were a computer right in front of you. For Linux instances, this usually involves using SSH (Secure Shell) with a command-line terminal to execute commands, manage files, install software, and configure the instance. For Windows instances, it typically involves using RDP (Remote Desktop Protocol) for a graphical desktop experience. This direct access is essential for server administration and application deployment.

### SSH is a key method for directly accessing a VM

SSH traffic means the secure, encrypted flow of data that occurs when using the Secure Shell (SSH) protocol to remotely access and manage a computer, like an EC2 instance. When you establish an SSH connection, it first authenticates you, often using a key pair to verify you have the correct private key corresponding to a public key on the server. Once connected, all data transmitted between your computer and the remote machine—including commands you type and the server's responses—is encrypted. This encryption makes SSH a standard and very secure method for administrative tasks, troubleshooting, or transferring sensitive information over potentially insecure networks. 

### To enable direct access, I set up key pairs

Key pairs are sets of cryptographic keys, consisting of a public key and a private key, that are used to securely prove your identity when connecting to an Amazon EC2 instance. When you launch an instance, you typically associate it with the public key. To log in, you must provide the corresponding private key. For Linux instances, this is primarily used for SSH (Secure Shell) access, acting like a very secure password that ensures only authorized users can gain remote access to the server.

A private key's file format means the specific way the private key information is encoded and stored in a file on your computer. Different tools and operating systems might expect different formats. My private key's file format was .pem (Privacy Enhanced Mail). This is a common format provided by AWS when you create a new key pair, and it's widely used with OpenSSH on Linux and macOS, or can be converted for use with tools like PuTTY on Windows.

---

## Launching a public server

I had to change my EC2 instance's networking settings by typically modifying its associated security group or by adjusting settings related to its network interface through the AWS Management Console or using the AWS CLI. For example, to control what traffic can reach the instance, I would edit the inbound rules of its security group. If I needed to change its ability to communicate with the internet, I might ensure it's in a public subnet with a public IP and that the VPC's route tables and internet gateway are correctly configured. The specific method depends on the exact networking aspect being modified. 

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-ec2_88727bef)

---

## Launching a private server

My private server has its own dedicated security group because it requires much stricter access controls compared to a public-facing server, especially for administrative access like SSH. While a public server might have rules allowing HTTP/HTTPS traffic from anywhere (0.0.0.0/0) for general accessibility, its SSH access should also be restricted. For a private server, which typically hosts backend services or databases, direct access from the internet is usually undesirable. Therefore, its security group is configured with more restrictive inbound rules. For instance, instead of allowing SSH from anywhere, its security group might only permit SSH connections originating from specific, trusted sources, like a bastion host in a public subnet or other instances within the same VPC that belong to a designated public security group. This enhances the overall security posture of the private resources.

My private server's security group's source is set to another security group, specifically the 'NextWork Public Security Group,' for its inbound SSH rule. which means that only EC2 instances (or other resources) that are themselves members of the 'NextWork Public Security Group' are permitted to initiate SSH connections to my private server. This is a powerful security practice because it tightly restricts access to a known and controlled set of resources within my VPC (like a bastion host or web servers in the public subnet that need to communicate with the private server), rather than allowing SSH access from any IP address on the internet (0.0.0.0/0) or even a broad IP range. This greatly reduces the attack surface for the private server.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-ec2_4a9e8014)

---

## Speeding up VPC creation

I used an alternative way to set up an Amazon VPC! This time, I selected the 'VPC and more' option in the AWS Management Console instead of just 'VPC only'. This presented a visual VPC resource map that showed the architectural layout of all the components being created—like the VPC itself, subnets, route tables, and internet gateway—all on a single configuration page. I used the name tag auto-generation feature by entering "nextwork" to easily identify all related resources. I then configured the number of Availability Zones, specified the quantity of public and private subnets, adjusted their CIDR blocks (e.g., to 10.0.0.0/24 for public and 10.0.1.0/24 for private), and chose to exclude NAT gateways and VPC endpoints for this initial setup to keep things simple and free. Finally, I clicked "Create VPC" to provision all these resources together. 

A VPC resource map is a visual diagram provided within the AWS VPC creation wizard (when using the 'VPC and more' option) that dynamically displays the architectural layout of your VPC and its associated resources. It clearly shows components like your subnets (public and private), route tables, and internet gateways, and illustrates how they are interconnected—for example, which subnets are linked to which route tables and which route tables have a path to an internet gateway. This visual representation is incredibly helpful for understanding the network design at a glance, making it easier to configure, manage, and troubleshoot your VPC architecture, especially as setups become more complex. It updates in real-time as you adjust configuration options like the number of subnets or Availability Zones.

My new VPC has a CIDR block of 10.0.0.0/16 (as an example often pre-filled by AWS). It is possible for my new VPC to have the same IPv4 CIDR block as my existing VPC because AWS VPCs are, by default, logically isolated environments within my account, even if they are in the same region. This isolation means that using the same private IP address range in two different VPCs won't cause immediate IP address conflicts between them. However, it's important to note that while this is technically possible, it's generally a best practice to use unique CIDR blocks for each VPC to avoid complications if you ever need to connect these VPCs later using services like VPC peering, as overlapping CIDRs would prevent direct routing between them.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-ec2_1cbb1b88)

---

## Speeding up VPC creation

### Tips for using the VPC resource map

When determining the number of public subnets in my VPC using the 'VPC and more' wizard, I only had two options: 0 or 2 (when 2 Availability Zones were selected). This was because AWS guides you towards best practices for high availability and redundancy. If you choose to have public subnets (and have selected, for example, two Availability Zones for your VPC), the wizard ensures you create one public subnet in each of those Availability Zones. This design helps ensure that your public-facing resources remain accessible even if one Availability Zone experiences an outage. The wizard restricts creating just one public subnet (if multiple AZs are in play for the VPC) because that would not offer the same level of fault tolerance. While you can manually add more later, the wizard simplifies the initial setup by promoting this resilient design. 

The set up page also offered to create NAT gateways, which are managed AWS services that enable instances in a private subnet to initiate outbound connections to the internet (for example, to download software updates or patches) while preventing unsolicited inbound connections from the internet from reaching those private instances. Unlike an internet gateway which allows two-way communication for public subnets, a NAT gateway acts as an intermediary. Instances in the private subnet send their outbound traffic to the NAT gateway (which resides in a public subnet and has an Elastic IP address), and the NAT gateway then forwards this traffic to the internet using its own public IP. This means your instances in the private subnet do not need public IP addresses themselves, keeping their private IPs hidden and significantly reducing their exposure to external threats. They cost money, which is why I opted out for this initial setup.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-networks-ec2_8ee57662)

---

---
