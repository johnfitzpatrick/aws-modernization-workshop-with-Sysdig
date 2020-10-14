---
title: "1.5 Push and scan an image from the registry"
chapter: false
weight: 32
---

### 1.5.1 Download Example Dockerfile and Sources

To illustrate the images scanning we will build an example Node.JS application based on the official “hello world” example described in [[their website](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/)](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/).

1. Download and uncompress example container files


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

	This is an example output of the previous two commands:

		```bash 
		$ docker build . -t $IMAGE --no-cache
		Sending build context to Docker daemon   5.12kB
		Step 1/7 : FROM node:12
		 ---> 28faf336034d
		Step 2/7 : WORKDIR /usr/src/app
		 ---> Running in 93b920dec3be
		Removing intermediate container 93b920dec3be
		 ---> 01e5e1af32fe
		Step 3/7 : COPY package*.json ./
		 ---> 44810837dce9
		Step 4/7 : RUN npm install
		 ---> Running in ca99accc9eb2
		npm notice created a lockfile as package-lock.json. You should commit this file.
		npm WARN docker_web_app@1.0.0 No repository field.
		npm WARN docker_web_app@1.0.0 No license field.

		added 50 packages from 37 contributors and audited 50 packages in 1.536s
		found 0 vulnerabilities

		Removing intermediate container ca99accc9eb2
		 ---> f26f400e82a4
		Step 5/7 : COPY . .
		 ---> 6056296396ae
		Step 6/7 : EXPOSE 8080
		 ---> Running in e952a4b076b0
		Removing intermediate container e952a4b076b0
		 ---> b3a0bdaa0db9
		Step 7/7 : CMD [ "node", "server.js" ]
		 ---> Running in 65d059bfd652
		Removing intermediate container 65d059bfd652
		 ---> 348910932015
		Successfully built 348910932015
		Successfully tagged 972909301756.dkr.ecr.us-east-1.amazonaws.com/aws-workshop:latest

		$ docker push $IMAGE                                                 
		The push refers to repository [972909301756.dkr.ecr.us-east-1.amazonaws.com/aws-workshop]                                       
		1fe0b71caae9: Pushed                                                                                                            
		92b0b058635c: Pushed                                                                                                            
		a9054618fca1: Pushed                                                                                                            
		d6edb296dc25: Pushed                                                                                                            
		fee9b925cc06: Pushed                                                                                                            
		a447e65f1e5f: Pushed                                                                                                            
		378725267e28: Pushed                                                                                                            
		174e334f3f46: Pushed                                                                                                            
		cbe6bbd0c86f: Pushed                                                                                                            
		ef5de533cb53: Pushed                                                                                                            
		a4c504f73441: Pushed                                                                                                            
		e8847c2734e1: Pushed                                                                                                            
		b323b70996e4: Pushed                                                                                                            
		latest: digest: sha256:bf4355d10ae38529cad32c81ad8e8c33ac0eac8dc41a8a4b90384621ff8e3853 size: 3048 
		```

3. Check that an image scan is automatically triggered ![Trigger Scan](/images/30_module_1/triggerscan.png)

4. Once complete you will see that this image has some issues ![Stack details](/images/30_module_1/scannissues.png)

5. As soon as the image finishes being pushed to the registry, a new **Amazon CodeBuild pipeline** will be automatically created that executes an image scan for it.</br>You don’t have to do anything about that, but you can check that the CodeBuild pipeline status by visiting: [https://console.aws.amazon.com/codesuite/codebuild/projects](https://console.aws.amazon.com/codesuite/codebuild/projects) ![Stack details](/images/30_module_1/codebuild.png)


Click on “InlineSecure Scanning” build project. ![JohnImagePendingTODO](/images/30_module_1/johnImagePending.png)


### 1.5.2 See scan results on Sysdig Secure dashboard

[Instructions how to get to scan results and search the image]

As you can see, the image have several major vulnerabilities.

![Sysdig Secure](/images/30_module_1/securescann.png)

[Explanation of the benefits of a single source of truth, and some Sysdig marketing stuff: policies, stopping gates, misconfigurations]