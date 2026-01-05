---
title: "Configure S3 Event Trigger"
date: 2026-01-05
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

#### Configure S3 Event Trigger

1. Open your Lambda function in the console.
2. Choose **Add trigger** and select **S3** as the source.

![add trigger](/images/5-Workshop/5.3-Training-flow/add-trigger.png)

In Trigger configuration:
- **Bucket**: choose the bucket you created earlier
- **Event type**: PUT
- **Prefix**: `data/train/` (only trigger on uploads to this folder)
- **Suffix**: `.csv` (only trigger for CSV files)

![trigger config](/images/5-Workshop/5.3-Training-flow/trigger-config.png)

Check the acknowledgement box and click **Add**.

![trigger added](/images/5-Workshop/5.3-Training-flow/trigger-added.png)

---

### Test the workflow

1. Create a test file `test-train.csv` locally.
2. Upload the file to the S3 path `/data/train/` using the S3 console or CLI.

![s3](/images/5-Workshop/5.3-Training-flow/choose-upload.png)

After a successful upload:

![s3](/images/5-Workshop/5.3-Training-flow/uploaded.png)

3. Open SageMaker Studio / Console → **Model training & customization** → **Training & tuning jobs**. You should see a new Training Job in **InProgress** state. This confirms the Event-Driven workflow triggered the training job successfully.

![sagemaker](/images/5-Workshop/5.3-Training-flow/job-created.png)













