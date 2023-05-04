+++
title = "b. Create IAM Role"
date = 2019-09-18T10:46:30-04:00
weight = 140
tags = ["tutorial", "install", "AWS", "Batch"]
+++

In this step, we will create an IAM role for Amazon ECS Task Execution.

AWS Batch uses Amazon ECS to create the compute environment. The task execution role grants the Amazon ECS container permission to make AWS API calls on your behalf.


Run the following commands in your Cloud9 terminal to create a task execution IAM role.

1. Create a file named ```ecs-tasks-trust-policy.json``` that contains the trust policy to use for the IAM role as below: 

```bash
cat > ecs-tasks-trust-policy.json << EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
```

2. Create an IAM role named **ecsTaskExecutionRole** using the trust policy created in the previous step. 

```bash

aws iam create-role --role-name ecsTaskExecutionRole --assume-role-policy-document file://ecs-tasks-trust-policy.json

```

3. Attach the AWS managed **AmazonECSTaskExecutionRolePolicy** policy to the **ecsTaskExecutionRole** role. This policy provides the permissions required to pull the container image from Amazon ECR private repository and to send the container logs to CloudWatch.

```bash
aws iam attach-role-policy  --role-name ecsTaskExecutionRole  --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy

```

You will be using this role when creating the AWS Batch Job definition later in this lab.

{{% notice info %}}

{{% /notice %}}

