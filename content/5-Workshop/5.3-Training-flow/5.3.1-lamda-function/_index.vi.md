---
title : "Tạo lambda function"
date: 2026-01-05
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions)
2. Trong thanh điều hướng, chọn **Function**, click **Create Function**:


![lambda](/images/5-Workshop/5.3-Training-flow/create-lambda.png)

3. Trong Create function console:
+ Đặt tên cho endpoint: Trigger-Training-Job
+ Chọn Runtime: Python 3.14 
+ Ở phần permission chọn **Use an existing role** và chọn role vừa tạo ban đầu

![lambda](/images/5-Workshop/5.3-Training-flow/info-function.png)

![lambda](/images/5-Workshop/5.3-Training-flow/choose-role.png)

Nhấn **Create function** và Lambda sẽ được tạo

![lambda](/images/5-Workshop/5.3-Training-flow/lambda-created.png)

*Trước đó hãy update policy của role để Lamda có thể sử dụng.*

Vào IAM console mục **Role** chọn role vừa tạo.

Ở phần **Trust relationship** nhấn **Edit trust policy** sau đó thêm statement cho phép Lambda vào.

![lambda](/images/5-Workshop/5.3-Training-flow/update-policy.png)

Nhấn **Update policy** và policy cho role sẽ được update

![lambda](/images/5-Workshop/5.3-Training-flow/role-updated.png)

Sau khi tạo function xong thì dán mã sau vào source code

```
import boto3
import os
import time

sm = boto3.client('sagemaker')

def lambda_handler(event, context):
    # 1. Parse thông tin từ sự kiện S3
    record = event['Records'][0]
    bucket = record['s3']['bucket']['name']
    key = record['s3']['object']['key']
    
    # 2. Định nghĩa tên Job duy nhất
    job_name = f"serverless-mlops-{int(time.time())}"
    
    # 3. Cấu hình Training Job
    # Sử dụng Scikit-learn container có sẵn của AWS
    image_uri = "683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3"
    
    role = os.environ.get('ROLE_ARN') # Cần set biến môi trường này
    
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

Nhấn **Deploy** 

Nhấn qua tab **Configuration**　để thêm **Enviroment variabled** 

Click **Edit** sau đó **Add enviroment variables** thêm:
+ Key: ROLE_ARN
+ Value: là ARN của role ở trên
Sau đó nhấn **Save**

![lambda](/images/5-Workshop/5.3-Training-flow/add-env.png)

