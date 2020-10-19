---
title: "Attach the IAM role to your Workspace"
chapter: false
weight: 22
---

1. Follow [this deep link to find your Cloud9 EC2 instance](https://console.aws.amazon.com/ec2/v2/home?#Instances:tag:Name=aws-cloud9-.*workshop.*;sort=desc:launchTime)
1. Select the instance, then choose **Actions / Instance Settings /Modify IAM Role**
![c9instancerole](/images/c9instancerole.png)
1. Choose **Sysdig-Workshop-Admin** from the **IAM Role** drop down, and select **Apply**

![c9attachrole](/images/c9attachrole.png)
https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:sort=desc:launchTime
