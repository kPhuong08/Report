---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Automate MLOps
## Automated Fine-tuning and Serverless Deployment Pipeline on AWS

### 1. Executive Summary
The Serverless MLOps Pipeline project is designed to address the challenges of cost optimization and the automation of Machine Learning model training (fine-tuning) and deployment. The system focuses on eliminating manual toil for data engineers by shifting from traditional infrastructure management to an Event-driven and Serverless architecture on AWS.

This MLOps workflow allows for automatic re-training triggers when new data arrives in S3, performs automated evaluation, and deploys the model to SageMaker Serverless Inference. The solution minimizes operational costs by leveraging EC2 Spot Instances for training and the "scale-to-zero" capability of Serverless Endpoints for inference.

### 2. Problem Statement
### What’s the Problem?
The current process for developing and operating ML models faces several obstacles:
- Manual & Disjointed: Engineers must manually run commands to train, evaluate, and deploy. The code base is difficult to maintain.
- Resource Waste: Utilizes expensive on-demand instances often left running after training completes. Endpoints run 24/7 regardless of traffic.
- Lack of Quality Control: Evaluation logic is rudimentary or mocked, leading to risks when deploying poor-quality models to production.

### The Solution
Construct a complete CI/CD/CT (Continuous Training) pipeline:
- Training: Use SageMaker Training Jobs with Spot Instances to reduce training costs (50-80% savings).
- Evaluation: Integrate real evaluation scripts that compare metrics (Accuracy, F1) against thresholds before accepting a model.
- Inference: Transition to SageMaker Serverless Endpoints, which only charge per request and are free when idle.
- Automation: Use Amazon EventBridge and GitHub Actions to orchestrate the workflow from data ingestion to deployment.

### Benefits and Return on Investment
The solution delivers clear economic efficiency and frees up AI/ML engineers. Estimated operational costs are only around $3.30/month for a standard scenario (compared to tens of dollars with static infrastructure). The time-to-market from new data to an updated model is reduced from days to hours, fully automated. CloudWatch Monitoring ensures immediate detection of training or inference issues.

### 3. Solution Architecture
The platform applies an Event-driven Serverless architecture to manage the Machine Learning model lifecycle. Training data (CSV/JSON) uploaded to S3 triggers an automated event chain involving training, evaluation, and deployment.

![IoT Weather Station Architecture](/images/2-Proposal/edge_architecture.jpeg)

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

### AWS Services Used
- Amazon S3: Stores Datasets, Model Artifacts (model.tar.gz), and Training Metrics.
- Amazon SageMaker:
  - Training Jobs: Runs Fine-tuning with HuggingFace Trainer (using Spot Instances).
  - Serverless Inference: Hosts model endpoints with auto-scaling capabilities (cold start 1-3s).
- AWS Lambda: Serverless function that executes the endpoint redeployment logic when a new model is available.
- Amazon EventBridge: Captures state change events (S3 upload, Training success) to orchestrate the flow.
- Amazon CloudWatch: Dashboard for monitoring training metrics, endpoint latency, and error rates.

### Workflow
- Ingest: User uploads the prepared dataset to Amazon S3.
- Trigger: An S3 Event triggers the process (via EventBridge or Webhook to GitHub Actions).
- Train: GitHub Actions initiates a SageMaker Training Job (using a HuggingFace container on a Spot Instance).
- Evaluate: Upon training completion, the pipeline runs a script to download the model and check metrics. If it meets the threshold, the model is saved as a candidate.
- Deploy: A "New Model" event triggers AWS Lambda, which updates the SageMaker Serverless Endpoint.

### 4. Technical Implementation
**Implementation Phases**
This project has two parts—setting up weather edge stations and building the weather platform—each following 4 phases:
- Build Theory and Tests: Research related services and manual test on console 
- Calculate Price and Check Practicality: Estimate costs and check that services are support what we need or not.
- Fix Architecture for Cost or Solution Fit: Adjust and change (model, services,..) to stay cost-effective and usable.
- Develop, Test, and Deploy: Configure, write scripts, build infrastructure.
  - Clean & Refactor: Cleanup legacy code, standardize directory structure (src, ops, lambda).
  - Data Preparation Script: Write prepare_data.py to split train/test sets and upload to S3.
  - Training Implementation: Configure train.py using HuggingFace Trainer, integrating metrics.json saving.
  - Evaluation Logic: Write evaluate_model.py to parse metrics and decide Pass/Fail based on thresholds.
  - CI/CD Integration: Configure GitHub Actions workflows to connect the above steps.
  - Serverless Deployment: Write the redeploy_endpoint Lambda function using Boto3 to update the SageMaker Endpoint.
  - Infrastructure as Code: Use Terraform to provision AWS resources (S3, IAM Roles, Lambda).

**Technical Requirements**
- Model: HuggingFace models (e.g., DistilBERT) for NLP tasks.
- Compute: Training uses ml.g4dn.xlarge (Spot); Inference uses Serverless memory config (1024MB-3072MB).
- Latency: Acceptable Cold Start of 1-3s for the first request after idle time.

### 5. Timeline & Milestones
**Project Timeline**
- Month 1: Study AWS and research.
- Month 2: Test and adjust.
- Month 3: Implement, test, and launch.
- Post-Launch: Research for the development direction.

### 6. Budget Estimation
You can find the budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=d233d205f0008133dda74d0a8ba715346e5cc658).  

### Infrastructure Costs
- AWS Services:
  - Training (SageMaker Spot):**~$3/month**.
  - Inference (SageMaker Serverless): **$3/month**.
  - Lưu trữ (Amazon S3): **~$0.3/month**.
  - CloudWatch: **~$5/tháng**
  - Orchestration (Lambda + EventBridge): **~$0.01/month** (likely free in Free Tier).

*Total*: ~$11 USD / month.


### 7. Risk Assessment
#### Risk Matrix
- Spot Instance Interruption: Training interrupted mid-way due to AWS reclaiming the instance.
  - Mitigation: Implement Checkpointing in the training code to resume from the last saved state instead of restarting.
- Cold Start Latency: The first request takes 1-3 seconds, affecting strict real-time user experiences.
  - Mitigation: Acceptable for internal tasks or non-critical real-time (sub-millisecond) requirements. Provisioned Concurrency can be used if necessary (higher cost).
- Model Drift: The newly trained model performs worse than the existing one.
  - Mitigation: The evaluate_model.py script acts as a gatekeeper, preventing deployment if metrics are lower than the current version.

### 8. Expected Outcomes
- 100% Automation: Zero manual intervention from data ingestion to live model.
- Cost Optimization: Reduces infrastructure costs to the absolute minimum for an ML project.
- Scalability: The architecture is easily adaptable to other ML problems (Computer Vision, Tabular data) by simply swapping the training script.
- Full Monitoring: comprehensive Dashboards regarding the health of training and serving systems.