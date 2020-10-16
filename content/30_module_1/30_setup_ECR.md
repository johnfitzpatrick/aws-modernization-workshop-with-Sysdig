---
title: "1.3. Setup Amazon ECR Registry"
chapter: false
weight: 30
---

**NOTE: Can we do this instead of UI?**
```sh
aws ecr create-repository --repository-name aws-workshop  --image-scanning-configuration scanOnPush=true
```

The output will be as follows

```JSON
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:845151661675:repository/aws-workshop",
        "registryId": "845151661675",
        "repositoryName": "aws-workshop",
        "repositoryUri": "845151661675.dkr.ecr.us-east-1.amazonaws.com/aws-workshop",
        "createdAt": 1602848100.0,
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": true
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}
```

1. Browse to [https://console.aws.amazon.com/ecr/get-started](https://console.aws.amazon.com/ecr/get-started),

2. For the purposes of this exercise we will be using AWS Region us-east-1. Select **'US East (N. Virginia)** us-east-1' from the 'Select a Region' dropdown on the top right

3. Click “Get Started” ![ECR](/images/30_module_1/ecr.png)

4. Give your repository an appropriate name, and enable “**Scan on push**”. The other setting may be left as default ![Repo name](/images/30_module_1/name_repo.png)

5. Click '**Create repository**'

6. Please make a note of the URI as you will require this in a later step. ![URI](/images/30_module_1/uri.png)


#### 1.3.1 Set up Credentials in Command line Docker

Shortly you will use your Cloud9 workspace to create and push a docker container to your new ECR Repository, however, before doing so you must configure docker's access to the repository.



1. Log into your Cloud9 workspace

2. Authenticate the Docker command line tool to this Amazon ECR registry, using AWS CLI tool as follows

```bash
export ECR_NAME=aws-workshop
export REGION=us-east-1
export AWS_ACCOUNT=$(aws sts get-caller-identity | jq '.Account' | xargs)
echo "$ECR_NAME, $REGION, $AWS_ACCOUNT"
aws ecr get-login-password --region $REGION | \
  docker login --username AWS --password-stdin \
  $AWS_ACCOUNT.dkr.ecr.$REGION.amazonaws.com
```

<!-- <&lt;alternative for AWS_ACCOUNT using sed instead>>

```bash
export AWS_ACCOUNT=$(aws sts get-caller-identity | grep "UserId" | sed 's/^[^"]*"\([^"]*\)".*"\([^"]*\)".*/\2/')
``` -->

The output should look similar to the following

```bash
WARNING! Your password will be stored unencrypted in /home/ec2-user/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```


*Note* For more details on this procedure, please refer to [[Amazon ECR registries - Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html)
