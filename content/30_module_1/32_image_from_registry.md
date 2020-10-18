---
title: "Push and Scan an Image from the Registry"
chapter: false
weight: 32
---

### Download Example Dockerfile and Sources

To illustrate the images scanning we will build an example Node.JS application based on the official “hello world” example described in [[their website](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/)](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/).

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

3. Check that an image scan is automatically triggered ![Trigger Scan](/images/30_module_1/triggerscan.png)

4. Once complete you will see that this image has some issues ![Stack details](/images/30_module_1/scannissues.png)

5. As soon as the image finishes being pushed to the registry, a new **Amazon CodeBuild pipeline** will be automatically created that executes an image scan.

	If you wish, you can check the CodeBuild pipeline status by visiting: [https://console.aws.amazon.com/codesuite/codebuild/projects](https://console.aws.amazon.com/codesuite/codebuild/projects) ![Stack details](/images/30_module_1/codebuild.png)

	**TRAINING NOTE: New ScreenShot Above and Below!!!**


Click on “InlineSecure Scanning” build project. ![JohnImagePendingTODO](/images/30_module_1/image1.png)


### See Scan Results on Sysdig Secure Dashboard

**TRAINING NOTE: Instructions here on how to get to scan results and search the image**

As you can see, the image have several major vulnerabilities.

![Sysdig Secure](/images/30_module_1/securescann.png)

**TRAINING NOTE: Explanation of the benefits of a single source of truth, and some Sysdig marketing stuff: policies, stopping gates, misconfigurations**
