---
title: "Static Website Using Cloudfront with S3 Bucket [Terraform]"
date: 2023-05-05 00:00:00 -0500
categories: [AWS, Infrastructure]
tags: [aws, cloudfront, s3, terraform, static-website, infrastructure-as-code]
---

This hands-on lab will walk you through how to set up a static website using an Amazon S3 bucket in combination with Amazon CloudFront. 

Preceding the lab, however, let us discuss what these components are and why they are used in combination with each other in relation to security.

Amazon S3
---
Amazon Simple Storage Service (S3 in short), is a highly durable (99.999999999%) and available (99.99%) object storage solution. S3 accepts almost any type of data and has limitless storage capacity. Furthermore, S3 provides a range of security features such as access management, storage encryption, versioning, and file integrity. The benefits discussed so far provide confidentiality, integrity, and availability of data, which covers all 3 CIA principles. Last but not least, S3 gives you the ability to host static websites.

Amazon CloudFront
---
CloudFront (CF) is Amazon’s content delivery network (CDN) offering. CloudFront ensures that your website’s content is located as close as possible to the end-user by replicating hosted files across various geo locations. As a result users benefit from high availability and superior loading speed which enhances the user’s experience. Amazon CF comes with numerous security features, such as encryption in transit (HTTPS), geo-location restrictions, DDoS protection, and more.  

So how do these two services work together?
---
TLDR; Static web content is hosted in your private S3 bucket that is only accessible by CloudFront. In this setup, CF acts as a proxy between the client user and the S3 bucket. CloudFront then retrieves (also caching) the content, and serves it to the end user. Using this configuration, the S3 bucket is not accessible by the public, which results in our files being secure. 

{% include youtube.html id="hjT30I4Xaro" %}
