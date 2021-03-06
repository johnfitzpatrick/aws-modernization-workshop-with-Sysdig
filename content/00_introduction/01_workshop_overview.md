+++
title = "Workshop Overview"
chapter = false
weight = 01
+++

It is extremely important to maintain a high level of security in your entire application environment. Not doing so can result in your system being compromised. This can incur significant costs and can lead to commercial & business issues, and failed compliance tests leading to a loss of trust with customers and monetary fines and/or settlement fees.  

In a cloud native environment, the security of your application is dependent largely upon the security of your containers, but not exclusively - it also depends upon the infrastructure upon which it runs.

In this workshop, you will learn how [[image scanning](https://sysdig.com/products/kubernetes-security/image-scanning/)](https://sysdig.com/products/kubernetes-security/image-scanning/) can provide the security insights you need without affecting the level of flexibility you desire.

![Image Scanning](/images/00_introduction/aliens.png)

In particular, we'll guide you on how to implement **ECS Fargate image scanning** with Sysdig Secure. The resulting solution will automatically scan any container image instance that is executed, and will warn you with reports about any vulnerabilities or misconfigurations in your workload. It will do this **without leaving your AWS workflow**, and without data leaving your AWS infrastructure.

We will also look at how to use Falco to perform 'runtime' security in the context of your AWS environment using Sysdig's CloudConnector's integration with AWS CloudTrail.
