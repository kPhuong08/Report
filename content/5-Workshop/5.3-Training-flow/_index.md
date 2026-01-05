---
title: "Build automated Training pipeline"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Building an automated Training pipeline

In this section, we will set up an event-driven mechanism. The goal is that when new training data is uploaded to an S3 bucket, a Lambda function will be automatically triggered. This Lambda acts as an "Orchestrator" to call SageMaker APIs and start a new Training Job without human intervention.

![overview](/images/5-Workshop/5.3-Training-flow/diagram.png)

#### Contents

- [Create Lambda function](5.3.1-lamda-function/)
- [Configure S3 Event & test](5.3.2-s3-event-trigger/)