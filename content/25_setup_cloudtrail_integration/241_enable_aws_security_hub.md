---
title: "Enable AWS Security Hub"
chapter: false
weight: 241
---

**TRAINING NOTE: Security Hub isn't a Sysdig thing, so in the interests of making things shorter, can we remove the individual steps/screenshots and replace them with this single command?**

To enable AWS Security Hub:

1. Log into your Cloud9 Workspace

2. Run the following command

```sh
aws securityhub enable-security-hub --enable-default-standards
```

**TRAINING NOTE: MAKE A GIF OF THIS STEPS!!! having all the pictures here adds little extra info but takes a lot of scrolling**

1. First make sure you are logged into your AWS account with your browser then browse to [AWS Security Hub](https://console.aws.amazon.com/securityhub/home). The screen you're presented with depends upon the status of your AWS account.

2. If you see this _“Get started with Security Hub”_ page below, then click '**Go to Security Hub**'. ![security_hub](/images/20_workshop_setup/security_hub.png)

3. Then, click '**Enable Standard**' in the dialog. ![enable](/images/20_workshop_setup/enable_standard.png)

4. However, if you see the Summary web page (shown later here), you can skip to the [Installing the CloudConnector](/20_workshop_setup/24_setup_cloudtrail/242_install_cloudconnector.html). ![summary](/images/20_workshop_setup/summary.png)

5. In the _"Welcome to AWS Security Hub"_ page, you can indicate which security standard controls you want to enable, or accept the default. <br /><br />***Note*** These controls are part of the default AWS Security Hub mechanism, and they are not related to the detections that Sysdig Cloud Connector is going to find for you.

6. Click '**Enable Security Hub**'. The _"Summary"_ page for Security Hub will be shown.  

{{% notice warning %}}
You may see a temporary red warning about AWS Config not being appropriately enabled, but it will disappear on its own once the Security Hub detects that the activation has been made. It has no relation to the use of Sysdig Cloud Connector.
![config_warning](/images/20_workshop_setup/config.png)
{{% /notice %}}
