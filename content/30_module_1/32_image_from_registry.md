---
title: "Push and Scan an Image from the Registry"
chapter: false
weight: 32
---

### Download Example Dockerfile and Sources

Now that our automatic scanner is in place, we can test it by pushing a Docker container, and check if it scans.

To illustrate the images scanning we will build an example Node.JS application based on the official “hello world” example described in [their website](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/).

1. Go to your Cloud9 Workspace and download and uncompress example container files


	```bash
	wget https://github.com/sysdiglabs/hello-world-node-vulnerable/releases/download/v1.0/hello-world-node-vulnerable.zip
	unzip hello-world-node-vulnerable.zip
	cd hello-world-node-vulnerable/
	```


2. And build and push the image to ECR

	```bash
	export IMAGE=$AWS_ACCOUNT.dkr.ecr.$REGION.amazonaws.com/$ECR_NAME
	docker build . -t $IMAGE
	docker push $IMAGE
	```

3. Check that an image scan is automatically triggered in [Amazon ECR](https://console.aws.amazon.com/ecr/repositories/aws-workshop/?region=us-east-1)

![Trigger Scan](/images/30_module_1/triggerscan.png)



4. Once complete you will see that this image has some issues ![Stack details](/images/30_module_1/scannissues.png)

5. As soon as the image finishes being pushed to the registry, a new **Amazon CodeBuild pipeline** will be automatically created that executes an image scan.

	If you wish, you can check the CodeBuild pipeline status by visiting: [Developer Tools > CodeBuild](https://console.aws.amazon.com/codesuite/codebuild/projects?region=us-east-1) ![Stack details](/images/30_module_1/CodeBuild-InProgress.png)

	If you like, you can drill down to tail the logs as the scan proceeds

	![Image Scan](/images/30_module_1/codebuild-01.png)


**TrainingNote - explanation here on this as phase one of the inline scan**

Once it transitions to "Succeeded", click on "InlineSecure Scanning" build project. ![Build Complete](/images/30_module_1/CodeBuild-ScanComplete.png)


### See Scan Results on Sysdig Secure Dashboard

**TrainingNote - explanation here that the scan metada is passed to Sysdig for analysis**

To see the scan results,

1. Log into the Sysdig Secure UI, and browse to 'Image Scanning > Scan Results'.

![Sysdig Secure](/images/30_module_1/Sysdig_Secure02.png)

2. Click your new `aws-workshop` image.

	You'll see the image have several major vulnerabilities.

![Sysdig Secure](/images/30_module_1/securescann02.png)


**TRAINING NOTE** AMAZON UI showing this as failed ![Scan Failed](/images/30_module_1/scanfailed.png)



**TRAINING NOTE: Explanation of the benefits of a single source of truth, and some Sysdig marketing stuff: policies, stopping gates, misconfigurations - Pawan will	 Supply**
