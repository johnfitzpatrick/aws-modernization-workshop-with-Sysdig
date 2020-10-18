---
title: "Installing the CloudConnector"
chapter: false
weight: 241
---

## Step 1. Enable AWS Security Hub

<!-- **TRAINING NOTE: Security Hub isn't a Sysdig thing, so in the interests of making things shorter, can we remove the individual steps/screenshots and replace them with this single command?** -->

To enable AWS Security Hub:

1. Log into your Cloud9 Workspace

2. Run the following command

    ```sh
    aws securityhub enable-security-hub --enable-default-standards
    ```
3. Log into your AWS account with your browser then browse to [AWS Security Hub](https://console.aws.amazon.com/securityhub/home).

{{% notice warning %}}
You may see a temporary red warning about AWS Config not being appropriately enabled, but it will disappear on its own once the Security Hub detects that the activation has been made. It has no relation to the use of Sysdig Cloud Connector.  
{{% /notice %}}![config_warning](/images/20_workshop_setup/config.png)


<!-- **TRAINING NOTE: MAKE A GIF OF THIS STEPS!!! having all the pictures here adds little extra info but takes a lot of scrolling**

1. First make sure you are logged into your AWS account with your browser then browse to [AWS Security Hub](https://console.aws.amazon.com/securityhub/home). The screen you're presented with depends upon the status of your AWS account.

2. If you see this _“Get started with Security Hub”_ page below, then click '**Go to Security Hub**'. ![security_hub](/images/20_workshop_setup/security_hub.png)

3. Then, click '**Enable Standard**' in the dialog. ![enable](/images/20_workshop_setup/enable_standard.png)

4. However, if you see the Summary web page (shown later here), you can skip to the [Installing the CloudConnector](/20_workshop_setup/24_setup_cloudtrail/242_install_cloudconnector.html). ![summary](/images/20_workshop_setup/summary.png)

5. In the _"Welcome to AWS Security Hub"_ page, you can indicate which security standard controls you want to enable, or accept the default. <br /><br />***Note*** These controls are part of the default AWS Security Hub mechanism, and they are not related to the detections that Sysdig Cloud Connector is going to find for you.

6. Click '**Enable Security Hub**'. The _"Summary"_ page for Security Hub will be shown.  

{{% notice warning %}}
You may see a temporary red warning about AWS Config not being appropriately enabled, but it will disappear on its own once the Security Hub detects that the activation has been made. It has no relation to the use of Sysdig Cloud Connector.
![config_warning](/images/20_workshop_setup/config.png)
{{% /notice %}} -->

## Step 2. Install the CloudConnector

To install this tool, we will be using a CloudFormation Template. Follow the steps below to install the Sysdig CloudConnector:

1. Navigate to the [[CloudFormation template for Sysdig Cloud Connector deployment](  https://console.aws.amazon.com/cloudformation/home#/stacks/create/template?stackName=CloudConnector&templateURL=https://cf-templates-cloud-connector.s3.amazonaws.com/cloud-connector.template)]( https://console.aws.amazon.com/cloudformation/home#/stacks/create/template?stackName=CloudConnector&templateURL=https://cf-templates-cloud-connector.s3.amazonaws.com/cloud-connector.template) template. The template will preview in CloudFormation. ![cf1](/images/20_workshop_setup/cf1.png)

2. On the “**Create stack**” section, click the '**Next**' button to start setting up the template. ![cf2](/images/20_workshop_setup/cf2.png)

3. The _“Specify stack details”_ section has no parameters for you to configure, so you can just press the **Next** button. ![cf3](/images/20_workshop_setup/cf3.png)

4. On _“Configure stack options_” screen, press the **Next** button. You can optionally add tag keys and values to the deployment, but no further configuration is required. Finally, you will be presented with a summary of all the parameters you previously introduced. Please note that dedicated IAM roles will be created to perform the scanning. These roles follow the "least privilege principle" to enforce maximum security. ![cf4](/images/20_workshop_setup/cf4.png)

5. Once you are happy with the plan, acknowledge it by marking the checkbox, and then press the **Create stack** button. ![cf5](/images/20_workshop_setup/cf5.png)

<!-- DevNote: Update this screenshot to have consistent arrow -->

## All ready!

On the CloudFormation dashboard, you should see the template status is 'CREATE_IN_PROGRESS'.

![cf6](/images/20_workshop_setup/cf6.png)

The creation process may take up to ten minutes. In the meantime you can continue with this workshop. You can later revisit the CloudFormation section in AWS to check the status of the deployment. It will show as “CREATE_COMPLETE” once the deployment has completed successfully.

![cf7](/images/20_workshop_setup/cf7.png)
