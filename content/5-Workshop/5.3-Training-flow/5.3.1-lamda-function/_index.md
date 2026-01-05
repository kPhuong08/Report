---
title: "Create Lambda function"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

1. Open the [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
2. In the navigation pane, choose **Functions**, then click **Create function**.

![lambda](/images/5-Workshop/5.3-Training-flow/create-lambda.png)

3. In the Create function console:
- Name the function: `Trigger-Training-Job`
- Runtime: **Python 3.14**
- Permissions: choose **Use an existing role** and select the role you created earlier.

![lambda](/images/5-Workshop/5.3-Training-flow/info-function.png)

After clicking **Create function**, the Lambda function will be created.

![lambda](/images/5-Workshop/5.3-Training-flow/lambda-created.png)

Before using the function, update the role's policy to allow Lambda to assume the role.

Go to IAM Console → **Roles** → select the role you created, then edit the **Trust relationship** and add the statement allowing Lambda to assume the role.

![lambda](/images/5-Workshop/5.3-Training-flow/update-policy.png)

Once the function is created, paste the following code into the function source:

```
import boto3
import os
import time

sm = boto3.client('sagemaker')

def lambda_handler(event, context):
    # 1. Parse information from the S3 event
    record = event['Records'][0]
    bucket = record['s3']['bucket']['name']
    key = record['s3']['object']['key']

    # 2. Define a unique job name
    job_name = f"serverless-mlops-{int(time.time())}"

    # 3. Configure the Training Job
    image_uri = "683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3"

    role = os.environ.get('ROLE_ARN')  # Set this environment variable in the Lambda configuration

    print(f"Starting training job {job_name} using data from s3://{bucket}/{key}")

    sm.create_training_job(
        TrainingJobName=job_name,
        HyperParameters={
            'sagemaker_program': 'train.py',
            'sagemaker_submit_directory': f"s3://{bucket}/code/source.tar.gz"
        },
        AlgorithmSpecification={
            'TrainingImage': image_uri,
            'TrainingInputMode': 'File',
        },
        RoleArn=role,
        InputDataConfig=[{
            'ChannelName': 'train',
            'DataSource': {
                'S3DataSource': {
                    'S3DataType': 'S3Prefix',
                    'S3Uri': f"s3://{bucket}/{key}",
                    'S3DataDistributionType': 'FullyReplicated'
                }
            }
        }],
        OutputDataConfig={'S3OutputPath': f"s3://{bucket}/output/"},
        ResourceConfig={'InstanceType': 'ml.m5.large', 'InstanceCount': 1, 'VolumeSizeInGB': 5},
        StoppingCondition={'MaxRuntimeInSeconds': 3600}
    )

    return {"statusCode": 200, "body": job_name}
```

![lambda](/images/5-Workshop/5.3-Training-flow/add-code.png)

Click **Deploy** to save the function.

Go to the **Configuration** tab to add environment variables:
- Key: `ROLE_ARN`
- Value: ARN of the role you created earlier

Click **Save**.

![lambda](/images/5-Workshop/5.3-Training-flow/add-env.png)