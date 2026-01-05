---
title : "Clean up resources"
date: 2026-01-05
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Clean up resources
This is the most important step to ensure you do not incur ongoing AWS charges after the workshop. We remove resources in the reverse order from creation: Endpoint -> Model -> Data.

#### Delete SageMaker resources
*Replace the endpoint name with your own*

1. Delete the Endpoint
```
aws sagemaker delete-endpoint --endpoint-name demo-serverless-endpoint
# Or the endpoint name created by CLI
aws sagemaker delete-endpoint --endpoint-name demo-serverless-endpoint
```

2. Delete the Endpoint Configuration

```
aws sagemaker delete-endpoint-config --endpoint-config-name demo-serverless-config
# Also delete any console-created configs if present
aws sagemaker delete-endpoint-config --endpoint-config-name demo-serverless-config
```

3. Delete the Model

```
# List models to copy the exact name
aws sagemaker list-models

# Delete the model
aws sagemaker delete-model --model-name demo-serverless-model
```

### Delete Data and Lambda

1. Clean the S3 bucket
```
# Replace with your bucket name
export BUCKET_NAME="your-bucket-name"

# Remove all objects (code, data, model artifacts)
aws s3 rm s3://$BUCKET_NAME --recursive

# Remove the bucket
aws s3 rb s3://$BUCKET_NAME
```

2. Delete Lambda functions
+ Go to the Lambda console
+ Select the function to delete
+ Choose **Action** -> **Delete**

![delete](/images/5-Workshop/5.5-Cleanup/delete-function.png)

3. Delete CloudWatch Log Groups
+ Go to CloudWatch -> **Logs** -> **Log Management**
+ Select log groups to delete -> **Action** -> **Delete log group(s)**

![delete](/images/5-Workshop/5.5-Cleanup/delete-log.png)