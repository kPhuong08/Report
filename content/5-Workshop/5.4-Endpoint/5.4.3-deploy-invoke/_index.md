---
title: "Automated deployment and testing"
date: 2026-01-05
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

The previous section covered how to deploy a model and an endpoint on SageMaker manually. This section shows how to automatically deploy a new model as soon as it is uploaded to S3.

### Automatic flow to create an endpoint and deploy a model

1. Create a Lambda function

+ Click **Create function**

![lambda](/images/5-Workshop/5.4-Endpoint/create-lambda.png)

+ Name: `Trigger-Deployment`
+ Runtime: Python 3.14

![lambda](/images/5-Workshop/5.4-Endpoint/info-lambda.png)

+ Permission: use an existing role
+ Select the IAM role created in earlier steps
+ Click **Create function**

![lambda](/images/5-Workshop/5.4-Endpoint/lambda-created.png)

2. Create a trigger

+ Click **Add trigger**

![lambda](/images/5-Workshop/5.4-Endpoint/add-trigger.png)

+ Source trigger: **S3**
+ Bucket: the bucket that will contain the model
+ Event type: **PUT**
+ Prefix: `output/`
+ Suffix: `.tar.gz`

![lambda](/images/5-Workshop/5.4-Endpoint/info-trigger.png)

+ Click **Add**

3. Lambda code

Add the following code to the Lambda function (adjust environment variables and ARNs to your environment):

```python
import boto3
import time
import os
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

sm_client = boto3.client('sagemaker')

# CONFIG (recommended to put in Lambda Environment Variables)
# Container image URI for Scikit-Learn (change according to your region)
# You can get this with: sagemaker.image_uris.retrieve("sklearn", region="us-east-1", version="0.23-1")
CONTAINER_IMAGE_URI = "683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3"
ROLE_ARN = "arn:aws:iam::064197589739:role/SageMakerExecutionRole"  # Replace with your role ARN

def lambda_handler(event, context):
    try:
        # 1. Retrieve the uploaded file info from the S3 event
        s3_record = event['Records'][0]['s3']
        bucket = s3_record['bucket']['name'].strip()
        key = s3_record['object']['key']

        model_url = f"s3://{bucket}/{key}"

        # Create unique names based on timestamp
        timestamp = time.strftime('%Y-%m-%d-%H-%M-%S', time.gmtime())
        model_name = f"sklearn-model-{timestamp}"
        endpoint_config_name = f"endpoint-config-{timestamp}"
        endpoint_name = f"endpoint-serverless-{timestamp}"

        logger.info(f"Detected new model: {model_url}")

        # 2. Create SageMaker Model
        # Note: Your inference.py must be present in model.tar.gz or configured via environment variables
        create_model_response = sm_client.create_model(
            ModelName=model_name,
            PrimaryContainer={
                'Image': CONTAINER_IMAGE_URI,
                'ModelDataUrl': model_url,
                'Environment': {
                    'SAGEMAKER_PROGRAM': 'inference.py',
                    'SAGEMAKER_SUBMIT_DIRECTORY': model_url
                }
            },
            ExecutionRoleArn=ROLE_ARN
        )
        logger.info(f"Created Model: {model_name}")

        # 3. Create Endpoint Configuration (Serverless)
        create_config_response = sm_client.create_endpoint_config(
            EndpointConfigName=endpoint_config_name,
            ProductionVariants=[
                {
                    'VariantName': 'AllTraffic',
                    'ModelName': model_name,
                    'ServerlessConfig': {
                        'MemorySizeInMB': 2048,  # choose 1024, 2048, 3072...
                        'MaxConcurrency': 5      # max concurrent requests
                    }
                }
            ]
        )
        logger.info(f"Created Serverless Config: {endpoint_config_name}")

        # 4. Create Endpoint
        create_endpoint_response = sm_client.create_endpoint(
            EndpointName=endpoint_name,
            EndpointConfigName=endpoint_config_name
        )

        return {
            'statusCode': 200,
            'body': f"Deploying Endpoint: {endpoint_name}. This may take several minutes."
        }

    except Exception as e:
        logger.error(f"Error: {e}")
        raise e
```

+ Click **Deploy**

When training and tuning produce a model artifact in S3, this Lambda will automatically deploy the model and create the corresponding endpoint.

![endpoint](/images/5-Workshop/5.4-Endpoint/end-inservice.png)

### Test endpoint

Run the following command to invoke the endpoint:

```bash
aws sagemaker-runtime invoke-endpoint --endpoint-name endpoint-deploy-serverless --body '["testdata", "test"]' --content-type application/json output_file.json
```

Example response:

```json
{
    "status": "success",
    "prediction": {
        "name": "Dummy Model",
        "accuracy": 99.9
    },
    "input_received": ["testdata", "test"]
}
```



