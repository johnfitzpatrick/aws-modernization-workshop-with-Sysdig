---
title: "Checking Security Findings in AWS Security Hub"
chapter: false
weight: 53
---

You can check these events without leaving the AWS console. This is how findings reported by Sysdig Cloud Connector look in the AWS Security Hub:

1. Browse to [Security Hub](https://console.aws.amazon.com/securityhub/home) and click 'Findings' on the left.


![AWS Security Hub](/images/50_module_3/image5.png)

{{% notice warning %}}
Seems my Security Hub isn't configured correctly. Getting error instead of above

"CLOUDTRAIL_MULTI_REGION_NOT_PRESENT
A multi-region CloudTrail trail with the required configuration does not exist in the account"

![ERROR.png](/images/50_module_3/SecHub_ERROR.png)
{{% /notice %}}

2. Click the details of one of these findings to view all the information you need to take immediate action:

![Delete Encryption](/images/50_module_3/image2.png)


And they appear in JSON format in AWS CloudWatch log streams:


![AWS CloudWatch Log Streams](/images/50_module_3/image1.png)
