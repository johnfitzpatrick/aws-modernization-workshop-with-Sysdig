---
title: "1.4 Deploy the Amazon ECR Integration"
chapter: false
weight: 31
---

This integration enables the Amazon Elastic Container Registry (ECR) to automatically trigger an action to scan every new container that is pushed into the registry.

1. Log into your AWS Console and select **'US East (N. Virginia)** us-east-1' from the 'Select a Region' dropdown on the top right. For the purposes of this exercise we will be using AWS Region us-east-1

2. Navigate to this [CloudFormation](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=ECRImageScanning&templateURL=https://cf-templates-secure-scanning-ecr.s3.amazonaws.com/ecr-image-scanning.template) template. ![CloudFormation](/images/30_module_1/create_stack.png)

3. Click **Next**. ![Stack details](/images/30_module_1/stack_details.png)

4. For **'ScanningType'** make sure the default value of 'Inline' is selected

5. For '**SysdigSecureEndpoint**', enter your 'Sysdig Secure API Token' for the Sysdig Secure account you created earlier. You can find in your User Profile. ![API token](/images/30_module_1/sysdig_api.png)

6. Click '**Next**'. You will be presented with 'Configure stack options' page.

7. Click '**Next**' accepting the default configuration options. ![Default options](/images/30_module_1/default_opt.png)

8. Make sure you tick the box acknowledging that AWS CloudFormation might create IAM resources with custom names.

9. Click '**Create stack**'. </br>You can view the status of the deployment from the Amazon CloudFormation screen. ![ECR](/images/30_module_1/cf_status.png)</br>This deployment will create a new Amazon CloudBuild project to automatically scan container images pushed to ECR registries. To view your Amazon CloudBuild projects, browse to [https://console.aws.amazon.com/codesuite/codebuild/projects?region=us-east-1](https://console.aws.amazon.com/codesuite/codebuild/projects?region=us-east-1) ![ECR](/images/30_module_1/codebuild.png)

