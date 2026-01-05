---
title: "Configure Serverless Endpoint"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

### Create Endpoint Configuration

1. In the SageMaker Console, open **Deployment & inference**.
2. Under **Endpoint configurations**, click **Create endpoint configuration**.

![endpoint](/images/5-Workshop/5.4-Endpoint/create-end-config.png)

Configuration options:
- **Name:** `serverless-endpoint`
- **Type of endpoint:** **Serverless**

![endpoint](/images/5-Workshop/5.4-Endpoint/info-end-config.png)

Click **Create production variant** and select the model you created in the previous step.

Press **Save**.

![endpoint](/images/5-Workshop/5.4-Endpoint/production-variant.png)

Edit the Variant settings as needed:
- **Name:** `variant-serverless`
- **Max concurrency:** 10 (default; request an increase from AWS if you need higher concurrency)

![endpoint](/images/5-Workshop/5.4-Endpoint/info-production.png)

Click **Create endpoint configuration**.

![endpoint](/images/5-Workshop/5.4-Endpoint/end-config-created.png)

### Create Endpoint

1. In the SageMaker Console, open **Deployment & inference**.
2. Under **Endpoints**, click **Create endpoint**.

![endpoint](/images/5-Workshop/5.4-Endpoint/create-end.png)

Configuration options:
- **Name:** `endpoint-deploy-serverless`
- **Attach endpoint configuration:** Use an existing endpoint configuration (select the one you just created)

![endpoint](/images/5-Workshop/5.4-Endpoint/info-end.png)

Click **Create endpoint** to deploy the Serverless endpoint.

![endpoint](/images/5-Workshop/5.4-Endpoint/end-created.png)