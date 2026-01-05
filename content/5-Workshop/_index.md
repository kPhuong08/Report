---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Automating MLOps workflows on AWS using SageMaker (Serverless approach)

#### Overview

**Serverless MLOps** is a modern approach that reduces infrastructure management overhead and optimizes costs by leveraging AWS serverless and auto-scaling services.

In this workshop, we will build a closed-loop pipeline: from ingesting raw data into the cloud to preparing a model ready to serve users — all triggered automatically without maintaining continuously-running servers.

We will focus on the following Serverless components:
+ **SageMaker Serverless Inference** - The main feature in this lab. It enables deploying an ML model as an API endpoint that can scale to zero when idle, so you pay only for compute time used.
+ **AWS Step Functions** - Acts as the orchestrator, replacing traditional Jenkins/GitLab Runner servers to manage training and deployment workflows.
+ **Amazon EventBridge** - Event-driven triggers to detect new data on S3 and start retraining workflows automatically.

#### Contents

1. [Workshop overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Build automated training pipeline](5.3-Training-flow/)
4. [Deploy Serverless Endpoint](5.4-Endpoint/)
5. [Cleanup resources](5.5-Cleanup/)