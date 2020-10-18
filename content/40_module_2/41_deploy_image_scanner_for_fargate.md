---
title: "Deploy Sysdig Secure Automatic Image Scanner for Fargate"
chapter: false
weight: 41
---

To deploy the Sysdig image scanner for Fargate, we'll again use Amazon CloudFormation.  The procedure is identical to how we installed Amazon ECR Integration in the previous lab, so this time we'll use the AWS CLI instead.

***Note*** You can find instructions on using the CLI on the [[Sysdig Fargate scanning installation page](https://sysdiglabs.github.io/ecs-image-scanning/install.html)](https://sysdiglabs.github.io/ecs-image-scanning/install.html)


1. First we'll set a couple of environment parameters to simplify the actual `aws` command,

    ```
    APIToken="F4k3F4k3-F4k3-F4k3-F4k3-F4k3F4k3F4k3"

    CFURI="https://cf-templates-secure-scanning-ecs.s3.amazonaws.com/ecs-image-scanning.template"
    ```

2. Then run the the following AWS CloudFormation command (which uses those environment parameters)

    ```
    aws cloudformation create-stack \
    --stack-name ECSImageScanning \
    --template-body $CFURI \
    --parameters ParameterKey=ECSInlineSecureAPIToken,ParameterValue=$APIToken ParameterKey=ECSInlineScanningType,ParameterValue=Inline \
    --capabilities "CAPABILITY_NAMED_IAM"
    ```

    You can check the status of the CloudFormation task by browsing to the [CloudFormation UI]([https://console.aws.amazon.com/cloudformation/](https://console.aws.amazon.com/cloudformation/)).

![CloudFormation](/images/40_module_2/image3.png)

Wait until the CloudFormation task completes, which may take several minutes.

Once all stacks are created, you will be ready to deploy our ECS tasks in a Fargate cluster securely, as all images will be scanned automatically.  In the next steps we will see this scanning as it happens.
