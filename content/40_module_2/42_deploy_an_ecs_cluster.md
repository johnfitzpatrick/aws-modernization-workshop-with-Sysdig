---
title: "Deploy an ECS cluster using Fargate"
chapter: false
weight: 42
---

To illustrate the automatic scanning, we will now deploy a sample ECS cluster that scales using Fargate.

**TrainingNote - Do we need more details on whats going on here????**


1. Create a cluster configuration and create a cluster

    ```
    ecs-cli configure --cluster tutorial --default-launch-type FARGATE --config-name tutorial --region us-east-1

    ecs-cli up --cluster-config tutorial --ecs-profile tutorial-profile
    ```

    The output should show a VPC and two Subnets have been created:-

    ```
    INFO[0000] Created cluster                    cluster=tutorial region=us-east-1
    INFO[0000] Waiting for your cluster resources to be created...
    INFO[0000] Cloudformation stack status       stackStatus=CREATE_IN_PROGRESS
    INFO[0060] Cloudformation stack status       stackStatus=CREATE_IN_PROGRESS
    VPC created: vpc-046ed77edcd796e19
    Subnet created: subnet-045df8f58a51b2291
    Subnet created: subnet-0e4623283c4907ea7
    Cluster creation succeeded.
    ```


1. Set the names of the VPC and Subnets as an environment variables, as follows

    ```bash
    export VPC=vpc-046ed77edcd796e19
    export SUBNET1=subnet-045df8f58a51b2291
    export SUBNET2=subnet-0e4623283c4907ea7
    ```

    **Note** You can subsequently get these details from the [CloudFormation UI](https://console.aws.amazon.com/cloudformation/home)


    ![ECS Cluster](/images/40_module_2/image7.png)

**TRAINING NOTE: THE FOLLOWING STEPS SEEM ERROR PRONE. ALTERNATIVE FLOW BELOW**

2. Run the following command to retrieve the id of the default security group and allow inbound access on port 80

    ```
    export group_id=$(aws ec2 describe-security-groups --filters Name=vpc-id,Values=$VPC --region us-east-1 | jq '.SecurityGroups[0].GroupId' | xargs)

    aws ec2 authorize-security-group-ingress --group-id $group_id --protocol tcp --port 80 --cidr 0.0.0.0/0 --region us-east-1

    echo The security group ID is $group_id
    ```

3. Create `ecs-params.yml` file as follows, `replacing subnets and security group id`.

    ```
    cat <<- 'EOF' > "ecs-params.yml"
    version: 1
    task_definition:
      task_execution_role: ecsTaskExecutionRole
      ecs_network_mode: awsvpc
      task_size:
        mem_limit: 0.5GB
        cpu_limit: 256
    run_params:
      network_configuration:
        awsvpc_configuration:
          subnets:
            - "subnet-045df8f58a51b2291"
            - "subnet-0e4623283c4907ea7"
          security_groups:
            - "sg-07c8bf61105e07021"
          assign_public_ip: ENABLED
    EOF
    ```

4. Create `docker-compose.yaml` file as follows

    ```
    cat <<- 'EOF' > "docker-compose.yml"
    version: '3'
    services:
      web:
        image: amazon/amazon-ecs-sample
        ports:
          - "80:80"
        logging:
          driver: awslogs
          options:
            awslogs-group: tutorial
            awslogs-region: us-east-1
            awslogs-stream-prefix: web
    EOF
    ```

5. Deploy to the cluster:

    ```
    ecs-cli compose --project-name tutorial service up --create-log-groups --cluster-config tutorial --ecs-profile tutorial-profile
    ```

___
**TRAINING NOTE: ALTERNATIVE FLOW::Above steps are error prone. Maybe replace steps 1-5 above with a script (maybe pull it in from a guthub, or [gist](https://gist.github.com/johnfitzpatrick/d55097212d9bb4e1442383a5e3339b01) during Cloud9 Workspace setup?**

We'll use a custom deployment script that will

- Retrieve the id of the default security group for the VPC creates, and allows inbound access on port 80

- Create a `ecs-params.yml` file using the subnets and security group already retrieved

- Create a `docker-compose.yaml` to instantiate the image `amazon/amazon-ecs-sample`


1. First set the names of the VPC and Subnets as an environment variables, as follows

    ```bash
    export VPC=vpc-046ed77edcd796e19
    export SUBNET1=subnet-045df8f58a51b2291
    export SUBNET2=subnet-0e4623283c4907ea7
    ```

<!--
2. Retrieve and view the deployment script

```bash
curl -s https://gist.githubusercontent.com/johnfitzpatrick/d55097212d9bb4e1442383a5e3339b01/raw/90aa0dbb5b7e35277aea87fad12879e987f4c820/deploy-amazon-ecs-sample.sh > deploy-amazon-ecs-sample.sh

chmod +x deploy-amazon-ecs-sample.sh

cat -n deploy-amazon-ecs-sample.sh
```
-->

3. Now run the script `deploy-amazon-ecs-sample.sh`

    ```bash
    cd /home/ec2-user/environment
    ./deploy-amazon-ecs-sample.sh
    ```

6. Once completed you can see on the [Amazon ECS UI](https://console.aws.amazon.com/ecs/home?region=us-east-1#/clusters/tutorial/services)

![Cluster Tutorial](/images/40_module_2/image5.png)
