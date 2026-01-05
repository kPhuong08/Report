---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

### Automating MLOps on AWS using SageMaker with Serverless approach
**MLOps** 

A convergence of Machine Learning, DevOps, and Data Engineering. It constitutes a closed-loop, automated process encompassing stages from Data Ingestion, Training, and Evaluation to Deployment and Monitoring. The primary objective is to minimize manual intervention and accelerate the time-to-market for machine learning models.

**SageMaker AI** 

A comprehensive AWS platform designed to build, train, and deploy ML models. In this lab, we will leverage SageMaker's most advanced features:

+ **SageMaker Training:** Utilizes Spot Instances to significantly reduce training costs.

+ **SageMaker Serverless Inference:** Deploys model APIs without the need for server management, featuring automatic scaling to zero when idle (Cost Optimization).

+ **SageMaker Pipelines (or Step Functions):** Used to orchestrate and coordinate the entire workflow.

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)