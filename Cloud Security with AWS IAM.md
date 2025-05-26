<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

**Author:** Ammad Ahmed  
**Email:** ayaanahmedcoding@gmail.com

---

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_1c864649)

---

## Introducing Today's Project!

In this project, I will demonstrate how to manage user access and permissions for cloud resources by launching an EC2 instance and then using AWS Identity and Access Management (IAM) to control who is authenticated and authorized to access it. This will include creating IAM policies, user groups, and setting up an AWS Account Alias. I'm doing this project to learn from scratch how to create and configure EC2 instances, implement robust security controls using IAM policies and user groups, manage IAM users effectively, and customize an AWS account with an alias. The overall goal is to gain practical experience in securing AWS resources through proper authentication and authorization mechanisms.

### Tools and concepts

Services I used were Amazon EC2 and AWS IAM! Key concepts I learnt include IAM users, policies, user groups and account aliases. We also learn how to use the Policy Simulator and how JSON policies work. How to launch an instance, how to tag an instance, how to log in as another user.

### Project reflection

This project took me approximately 1.5 hours today. The most challenging part was understanding the IAM policy since it was written in JSON and contained multiple statements. It was most rewarding to see permission denied when my intern tried to delete the production instances - the IAM access Management worked!

---

## Tags

Tags are organizational tools, essentially key-value pairs, that let us label our AWS resources, such as EC2 instances. They are particularly useful for grouping related resources, managing cost allocation by tracking expenses, automating operational tasks (like starting/stopping instances with specific tags), and applying IAM policies consistently. Tags also significantly improve resource identification and filtering within the AWS Management Console or through scripts.

The tag I’ve used on my EC2 instances is called 'Env' to my two EC2 instances, which stands for 'environment'. For one EC2 instance, the value assigned to the 'Env' tag is 'production', and for the other EC2 instance, the value assigned is 'development'.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

IAM Policies are like rules that determine who can do what in my AWS Account. I'm using policies today to control who has access to my production/ enviroments instance.

### The policy I set up

For this project, I’ve set up a policy usinn JSON.

I’ve created a policy that allows the policy holder (i.e. the intern) to have permission to do anything they want to any instance tagged with "development". They can also see information for any instance, but they're denied access to deleting/creating  tags for any instance as well.

### When creating a JSON policy, you have to define its Effect, Action and Resource.

The Effect, Action, and Resource attributes of a JSON policy means whether or not the policy is allowing/denying action (i.e. Effect); what the policy holder can or cannot do (i.e. Action); and the specific AWS resoruces that the policy relates to (i.e. Resources).

---

## My JSON Policy

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_1c864649)

---

## Account Alias

An account alias is simply nickname for my AWS account! Instead of a long Account ID, we can new reference our account alias instead!

Creating an account alias took me 30 seconds - it's a simple configuration in the IAM dashboard. Now, my new AWS console sign-in URL uses the alias instead of my account ID.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### Users

IAM users are people or entities that have acces/can login to my AWS account. 

### User Groups

IAM user groups are like folders that collect IAM users so that you can apply permission settings at the group level.

I attached the policy I created to this user group, which means any user created inside this group will automatically get the permissions attached to the NextWorkDevEnviromentPolicy IAM policy.

---

## Logging in as an IAM User

The first way is to email sign-in instructions to the user, while the second way is to download a .csv file with the sign in details inside.

Once I logged in as my IAM user, I noticed that my user is already denied access to panels on the main AWS console dashboard. This was because I only set up permissions to my development EC2 instances, so my intern wouldn't have access to even see anything else.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

I tested my JSON IAM policy by attempting to stop both the development and the production instances.

### Stopping the production instance

When I tried to stop the production instance I was met with an error! This was because my production instance is tagged with the 'production' label, which is outside permission policy - interns are only allowed to do things to development instances.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_0e7a9d6a)

---

## Testing IAM Policies

### Stopping the development instance

Next, when I tried to stop the development instance, I succesfully saw the instance state change to Stopping and then Stopped. This was because my permission policy allows the intern (i.e. users in the nextwork-dev-group) to stop instances.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_1811801c)

---

## The IAM Policy Simulator

The IAM Policy Simulator is a tool that lets us simulate actions and test permission settings by defining a specific user/group/role and the actions we want to test for. It's useful for saving time when testing permission settings! No more logging into another user or actually stopping resources.

### How I used the simulator

I set up a simulation for wheather our dev user group has permission to StopInstances or DeleteTags. The results were denied for both - I had to adjust the scope of the EC2 instances to ones that are tagged with "Development". Once I applied that tag, permisssion was allowed.  

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-security-iam_069d8a621)

---

---
