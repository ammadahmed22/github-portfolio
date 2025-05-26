<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Host a Website on Amazon S3
[Home](./https://github.com/ammadahmed22/github-portfolio)

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-host-a-website-on-s3)

**Author:** Ammad Ahmed  
**Email:** iosammadahmed@gmail.com

---

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Introducing Today's Project!

In this project, we will demonstrate how to use S3 to host a static website. We're doing this project to learn about AWS and cloud services and how they can be used to store objects in the cloud AND even host websites.

### Tools and concepts

Services I used were Amazon S3. Key concepts I learnt include bucket policies , uploading static website files, index.html, bucket endpoint URLs and ACLs and how they control access to my bucket's objects.

### Project reflection

This project took me approximately 1.5 hours but this is including demo time, quiz time, and secret mission time. The most challenging part was resolving the 403 Forbidden error. It was most rewarding to see my website load live and to be public for the world to access.

---

## How I Set Up an S3 Bucket

Creating an S3 bucket took me less then 5 minutes, I wanted to learn a few new concepts like block public access and ACLs, but once I learned that I'm able to create buckets in shorter times.

The Region I picked for my S3 bucket was N.Virginia because its the Region that is closest to me. It's best pratice to pick the region closest to you because it lowers time to retrieve your things (aka latency), and cost 

S3 bucket names are globally unique! This means no two Amazon S3 buckets in the entire world can have the same name. They have to be completely unique, regradless of the region the account ID.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_ba6d42ad)

---

## Upload Website Files to S3

### index.html and image assets

I uploaded two files to my S3 bucket - they were an index.html file (this determines the structure i.e. what goes inside the website) and a folder of images and assests (this will fill in the website with images and things to look at.)

Both files are necessary for this project as index.html determines the structure, but the structure alone does not illustrate the contents of the website, i.e. if index.html says "insert image here", it might not have the acutally image to display so we need to supply that image separately, That's why we have multiple files uploaded - the index.html file to direct what is inside the webstie page, but also a bunch of assests (like images) that the website is trying to display.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_a265af88)

---

## Static Website Hosting on S3

Website hosting means putting the website files on a website server, which is a speical computer designed to turn the files into a website page that people can visit!

To enable website hosting with my S3 bucket, went into the propertires tab of our bucketm enabled static website hosting, and we also labelled "index.html" as main document. i.e. this the document that we're trying to host. 

An ACL is (aka an Access Control List) is a way to configure permission settings inside a bucket. We enrolled ACLs so that we can control access  our website files later. There was a popup mentioning that AWS recommends disabling ACLs, but keep it enabled to learn how ACLs work and compare it with bucket policies later.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_c22c54c0)

---

## Bucket Endpoints

Once static website is enabled, S3 produces a bucket endpoint URL, which is a URL that tables you (and anyone on the internet) to the website that you're hosting!

When I first visited the bucket endpoint URL, I saw a 403 error The reason for this error was  that objects in a  bucket are public by default - even though I switched off "Blocked all Public Access", the website file themselves are still completely private. So I need to manage it's  access settings separately - they need to be public files too for the public to see the contents of our website.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_22ce4daf)

---

## Success!

To resolve this 403 Forbidden error, I updated the access settings of the files inside my bucket too. Using ACLs, we made our bucket's files public! Once I checked my S3 bucket endpoint, I was abkle to see a webpage all loaded up :).

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Bucket Policies

An alternative to ACLs are bucket policies, which are rules that determine who is allowed (or NOT aollwed) to do something. The benefit of using bucket policies is that you can have even greater control of the ACTIONS that people are and are not aollowed to do, while ACLs are useful for controlling public access to individual objects inside the bucket.

My bucket policy denies everyone from deleting my index.html file in the bucket. I tested this by trying to delete index.html and saw permission denied error! This means my bucket policty worked it successfully stopped me from deleting the object I wanted to protect.

![Image](http://learn.nextwork.org/inspired_gold_shy_gazelle/uploads/aws-host-a-website-on-s3_sm2sm2sm)

---

---
