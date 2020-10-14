---
title: "1.6 Modify the image and trigger a second scan"
chapter: false
weight: 33
---

For illustration purposes, let's rebuild our image and make it more secure by starting with a different Base image. To do so:

1. Go back into Cloud9 Workspace
2. Edit the Dockerfile and in the first line update the base image `FROM node:12` to `FROM bitnami/node:12`.

		```
		FROM bitnami/node:12

		# Create app directory
		WORKDIR /usr/src/app

		# Install app dependencies
		# A wildcard is used to ensure both package.json AND package-lock.json are copied
		# where available (npm@5+)
		COPY package*.json ./

		RUN npm install
		# If you are building your code for production
		# RUN npm ci --only=production

		# Bundle app source
		COPY . .

		EXPOSE 8080
		CMD [ "node", "server.js" ]
		```

3. Now rebuild and push the image again with: 

		```
		docker build . -t $IMAGE
		docker push $IMAGE
		```

	The image will automatically be scanned, as before.

4. Once completed, you will see that the scan result now shows it doesn’t have any vulnerabilities. ![JohnImagePendingTODO2](/images/30_module_1/johnImagePending.png)

