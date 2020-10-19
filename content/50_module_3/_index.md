+++
title = "Module 3: CloudTrail Runtime Security"
chapter = true
weight = 50
+++

# Module 3: CloudTrail Runtime Security

## Module Overview

Every action taken over your infrastructure resources results in a registry in AWS CloudTrail. This includes all AWS account activity, actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. This event history is useful for detecting unwanted or unexpected activity involving your AWS resources, however as your infrastructure grows, the amount of events and operational trails can become so huge that analyzing them is no longer practical. However, failing to react to a threat quickly could potentially have major consequences.

In this module we will explain how to audit AWS CloudTrail events with Sysdig Cloud Connector.  Once deployed in your infrastructure, the Sysdig Cloud Connector analyzes every CloudTrail entry in real time, and provides AWS threat detection by evaluating each event against a flexible set of security rules based on Falco. This allows you to detect threats and raise notifications so you can address security threats as quickly as possible.


## Reference Architecture

Similar to ECS Fargate serverless, incoming CloudTrail events are fetched and stored in an S3 bucket. A subscription in the SNS topic will then forward the events to the **Sysdig Cloud Connector** endpoint. The Cloud Connector will then analyze each event against a configured set of **Falco rules**.

![Reference Architecture](/images/50_module_3/image6.png)

Sysdig Cloud Connector provides several **notification options**, including sending security findings to Sysdig Backend, as well as AWS Security Hub and AWS CloudWatch, so you can review the security events without leaving your AWS console.

![Reference Architecture](/images/50_module_3/image4.png)

A set of out-of-the-box Falco rules for CloudTrail is available in the Sysdig Cloud Connector, saving you a lot of time getting started, but you can complement these with your own custom rules to detect any cloud command.

The included out-of-the-box CloudTrail rules can detect events like:

*   Add an AWS user to a group.
*   Allocate a New Elastic IP Address to AWS Account.
*   Attach an Administrator Policy.
*   Create an AWS user.
*   Deactivate MFA for user access.
*   Delete bucket encryption.

These rules are also mapped against **NIST 800-190** security standard controls. More compliance mapping for additional security standards like PCI or CIS will be provided in the future
