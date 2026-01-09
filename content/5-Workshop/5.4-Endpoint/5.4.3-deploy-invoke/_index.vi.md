---
title : "Tự động deploy và kiểm thử"
date: 2026-01-05
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

Ở phần trước sẽ giúp ta hiểu thêm về cách deploy một model và endpoint trên Sagemaker ở phần này sẽ cho mọi người cách để có thể deploy một cách tự động khi có một model mới được thêm vào S3

### Tự động flow tạo endpoint và deploy model
1. Tạo Lambda function 
+ Click **Create function**

![lamda](/images/5-Workshop/5.4-Endpoint/create-lambda.png)

+ Name: Trigger-Deployment
+ Runtime: Python 3.14

![lamda](/images/5-Workshop/5.4-Endpoint/info-lambda.png)

+ Permission: use an existing role
+ Chọn role đã tạo ở những bước 
+ Nhấn **Create function**

![lamda](/images/5-Workshop/5.4-Endpoint/lambda-created.png)

2. Tạo trigger
+ Nhấn **Add trigger**

![lamda](/images/5-Workshop/5.4-Endpoint/add-trigger.png)

+ Source trigger: S3
+ Bucket: bucket chứa model
+ Event type: PUT
+ Prefix: output/
+ Subprefix: .tar.gz

![lamda](/images/5-Workshop/5.4-Endpoint/info-trigger.png)

+ Nhấn **Add**

3. Code Lambda

Thêm đoạn code sau vào code của Lambda

```python 
import boto3
import time
import os
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

sm_client = boto3.client('sagemaker')

# CẤU HÌNH (Nên để trong Environment Variables của Lambda)
# URI của container Scikit-Learn (Thay đổi theo region của bạn)
# Bạn có thể lấy link này bằng lệnh: sagemaker.image_uris.retrieve("sklearn", region="us-east-1", version="0.23-1")
CONTAINER_IMAGE_URI = "683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3"
ROLE_ARN = "arn:aws:iam::064197589739:role/SageMakerExecutionRole" # Thay bằng Role ARN của bạn

def lambda_handler(event, context):
    try:
        # 1. Lấy thông tin file vừa upload từ S3 Event
        s3_record = event['Records'][0]['s3']
        bucket = s3_record['bucket']['name'].strip()
        key = s3_record['object']['key']
        
        model_url = f"s3://{bucket}/{key}"
        
        # Tạo tên unique cho model và endpoint dựa trên thời gian
        timestamp = time.strftime('%Y-%m-%d-%H-%M-%S', time.gmtime())
        model_name = f"sklearn-model-{timestamp}"
        endpoint_config_name = f"endpoint-config-{timestamp}"
        endpoint_name = f"endpoint-serverless-{timestamp}"

        logger.info(f"Phát hiện model mới: {model_url}")

        # 2. Tạo SageMaker Model
        # Lưu ý: Script inference.py của bạn PHẢI nằm trong model.tar.gz hoặc cấu hình biến môi trường để trỏ tới
        create_model_response = sm_client.create_model(
            ModelName=model_name,
            PrimaryContainer={
                'Image': CONTAINER_IMAGE_URI,
                'ModelDataUrl': model_url,
                'Environment': {
                    'SAGEMAKER_PROGRAM': 'train.py', # Tên file script chính của bạn
                    'SAGEMAKER_SUBMIT_DIRECTORY': model_url # Nơi chứa code (thường chung với model artifact)
                }
            },
            ExecutionRoleArn=ROLE_ARN
        )
        logger.info(f"Đã tạo Model: {model_name}")

        # 3. Tạo Endpoint Configuration (SERVERLESS)
        create_config_response = sm_client.create_endpoint_config(
            EndpointConfigName=endpoint_config_name,
            ProductionVariants=[
                {
                    'VariantName': 'AllTraffic',
                    'ModelName': model_name,
                    'ServerlessConfig': {
                        'MemorySizeInMB': 2048, # Chọn RAM: 1024, 2048, 3072...
                        'MaxConcurrency': 5     # Số request tối đa xử lý cùng lúc
                    }
                }
            ]
        )
        logger.info(f"Đã tạo Config Serverless: {endpoint_config_name}")

        # 4. Tạo Endpoint
        create_endpoint_response = sm_client.create_endpoint(
            EndpointName=endpoint_name,
            EndpointConfigName=endpoint_config_name
        )
        
        return {
            'statusCode': 200,
            'body': f"Đang deploy Endpoint: {endpoint_name}. Quá trình này mất vài phút."
        }

    except Exception as e:
        logger.error(f"Lỗi: {e}")
        raise e
```
+ Nhấn **Deploy**
Sau khi Training & tunning tạo ra output là một model trong S3 thì sẽ tự động deploy model và tạo endpoint tương ứng

![endpoint](/images/5-Workshop/5.4-Endpoint/end-inservice.png)

### Test endpoint
1. Tạo 1 file input.json với data test bất kỳ

```
["testdata", "test"] 
```

2. Test endpoint

*Thay endpoint-name bằng endpoint của bạn*

``` 
aws sagemaker-runtime invoke-endpoint --endpoint-name endpoint-serverless-2026-01-09-06-04-49 --body fileb://input.json --content-type application/json output_file.json
```

Kết quả trả về sẽ nằm trong file output_file.json có dạng tương tự:
```
{
    "status": "success",
    "prediction": {
        "name": "Dummy Model",
        "accuracy": 99.9
    },
    "input_received": ["testdata", "test"]
}
```





