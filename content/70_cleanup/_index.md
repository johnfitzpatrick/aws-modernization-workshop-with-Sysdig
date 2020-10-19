+++
title = "Cleanup"
chapter = true
weight = 70
+++

## Cleanup
#### Module 3
- Delete the log group created to test the Falco rule modification

    ```
    aws logs delete-log-group --log-group-name "test_unused_region" --region="us-west-2"
    ```

- Delete an S3 bucket

    ```
    aws s3api delete-bucket --bucket $BUCKETNAME
    ```

#### Module 2

- Remove ECS Cluster

    ```
    ecs-cli compose service rm --cluster-config tutorial --ecs-profile tutorial-profile
    ecs-cli down --force --cluster-config tutorial --ecs-profile tutorial-profile
    ```

- Remove Image Scanner Integration for Fargate

    ```
    aws cloudformation delete-stack --stack-name ECSImageScanning
    ```

- Unattach the task execution role policy & delete role:

    ```
    aws iam --region us-east-1 detach-role-policy --role-name ecsTaskExecutionRole --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy

    aws iam --region us-east-1 delete-role --role-name ecsTaskExecutionRole
    ```


#### Module 1
- Remove container image from Amazon ECR Registry
    ```
    docker registry rmi $IMAGE
    ```

- Remove Amazon ECR Integration

    ```
    aws cloudformation delete-stack --stack-name ECSImageScanning
    ```

- Remove Amazon ECR Registry

    ```
    aws ecr delete-repository --repository-name aws-workshop --force
    ```


#### Introduction
  - Installing the CloudConnector CloudFormation Template

    ```
    aws cloudformation delete-stack --stack-name CloudConnector
    ```

  - Disable Security Hub

    ```
    aws securityhub disable-security-hub
    ```

<!-- ___

#### Delete images pushed and ECR registry

Go to ECR dashboard on AWS, and remove all repositories \
[https://console.aws.amazon.com/ecr/repositories?region=us-east-1](https://console.aws.amazon.com/ecr/repositories?region=us-east-1)


#### Delete ECS Fargate cluster

_[Use CloudFormation stack delete]_


#### Delete CloudFormation stacks (only if you are not going to use them)

Go to the CloudFormation dashboard on AWS, select and delete each of the stacks. \
[https://console.aws.amazon.com/cloudformation/home?region=us-east-1](https://console.aws.amazon.com/cloudformation/home?region=us-east-1)

[Insert screenshot with all stacks deployed when service role conflict is resolved]


#### Delete other resources

Delete the log group create to test the Falco rule modification

  ```
  aws logs delete-log-group --log-group-name "test_unused_region" --region="us-west-2"
  ```
____ -->
