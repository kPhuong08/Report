---
title: "Create Model"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

After the Training Job in section 5.3 completes, a `model.tar.gz` file will appear in the S3 bucket under the `output/` folder.

![job](/images/5-Workshop/5.4-Endpoint/job-completed.png)

This file is the model artifact. To serve inference requests we must deploy it to a compute environment.

In this lab we will use **SageMaker Serverless Inference**:
- Real-time Endpoint (traditional): runs continuously and incurs costs even when idle.
- Serverless Endpoint: scales to zero when idle; compute starts on demand and you pay only for actual processing time.

#### Create the model object
This step packages model and inference code so SageMaker can load them into a container.

1. In the SageMaker Console, open **Deployment & inference** in the navigation bar.
2. Under **Deployable models**, click **Create model**.

![model](/images/5-Workshop/5.4-Endpoint/create-model.png)

Configuration suggestions:
- **Model name:** `model-serverless`
- **IAM role:** choose the role you created in prerequisites

![model](/images/5-Workshop/5.4-Endpoint/info-model.png)

Container definition:
- **Provide model artifacts and inference image location**
- **Inference image:** `683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3` (Scikit-learn/Python runtime)
- **Model artifacts:** link to the `model.tar.gz` produced by the Training Job (e.g., `s3://<your-bucket>/output/<job-id>/model.tar.gz`)

![model](/images/5-Workshop/5.4-Endpoint/info-container.png)

Environment variables (to point SageMaker to your inference code):
- `SAGEMAKER_PROGRAM`: `train.py`
- `SAGEMAKER_SUBMIT_DIRECTORY`: S3 path to `source.tar.gz`

![model](/images/5-Workshop/5.4-Endpoint/info-env.png)

3. After configuring, click **Create model**.

![model](/images/5-Workshop/5.4-Endpoint/model-created.png)"



