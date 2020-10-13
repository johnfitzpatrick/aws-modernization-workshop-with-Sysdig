---
title: "Infrastructure Runtime Security"
draft: false
weight: 03
---

In the same way image scanning gives you visibility of vulnerabilities and threats pertaining specifically to an application's containers, infrastructure scanning gives visibility of potential issues emanating from the environment on which these containers run. 

AWS provides a rich environment upon which to base your application, but it's not without its risks.  There are many places where bad actors can create harm, for example exposing data by making S3 buckets public, deleting bucket encryption, disabling MFA for an account, adding/removing IAM policies.

Falco is an open source threat detection language that is widely used to detect and alert on runtime abnormalities.  This tool can also be used to detect changes within the AWS environment.
