---
title: "Deploy Serverless Endpoint"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Deploying a Serverless Endpoint

In this section, we will package the model artifact produced by training and deploy it to a SageMaker Serverless Inference endpoint. Unlike a real-time endpoint (which runs continuously), a Serverless Endpoint can scale to zero when idle, helping optimize cost for infrequently used workloads.

![endpoint architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)

#### Contents

- [Create Model](5.4.1-model-create/)
- [Configure Serverless Endpoint](5.4.2-serverless-endpoint/)
- [Deploy and Invoke](5.4.3-deploy-invoke/)


