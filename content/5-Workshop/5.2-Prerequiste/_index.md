---
title : "Prerequiste"
date: 2026-01-05
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions

Create an IAM Role named **SageMakerExecutionRole** and attach the following policies (at minimum):
- AmazonSageMakerFullAccess
- AmazonS3FullAccess
- CloudWatchLogsFullAccess
- AWSLambda_FullAccess

Save the Role ARN; you'll need it in later steps.

---

### Create an S3 Bucket
First, we need a place to store training data and model artifacts.

![create bucket](/images/5-Workshop/5.2-Prerequisite/create-bucket.png)

Create the bucket and enable versioning (leave other fields as default).

Create the following folders (you can create empty objects to ensure the folders exist):
- `data/train/` – where you upload files that will trigger training
- `data/test/` – test data
- `models/` – where SageMaker will store trained model artifacts
- `code/` – training code (e.g. `train.py`)

![create folder](/images/5-Workshop/5.2-Prerequisite/create-folder.png)

![folders](/images/5-Workshop/5.2-Prerequisite/folders.png)

---

### Training script & sample data
Prepare a simple training script locally to simulate training.

Example `train.py`:

```
import argparse
import os
import time
import joblib

# ---------------------------------------------------------
# PART 1: REQUIRED FUNCTIONS FOR SAGEMAKER ENDPOINT (INFERENCE)
# ---------------------------------------------------------

def model_fn(model_dir):
    """SageMaker calls this automatically when the endpoint starts. Load model from disk."""
    print(f"Loading model from: {model_dir}")
    model_path = os.path.join(model_dir, 'model.joblib')
    if os.path.exists(model_path):
        return joblib.load(model_path)
    else:
        raise FileNotFoundError(f"Model not found at: {model_path}")


def predict_fn(input_data, model):
    """(Optional) Prepare model prediction output for the endpoint"""
    return {"status": "success", "prediction": model, "input_received": input_data}

# ---------------------------------------------------------
# PART 2: TRAINING CODE
# ---------------------------------------------------------

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--train', type=str, default=os.environ.get('SM_CHANNEL_TRAIN'))
    parser.add_argument('--model-dir', type=str, default=os.environ.get('SM_MODEL_DIR'))
    args, _ = parser.parse_known_args()

    print("Starting training...")
    time.sleep(5)  # simulate training

    dummy_model = {"name": "Dummy Model", "accuracy": 99.9}
    save_path = os.path.join(args.model_dir, 'model.joblib')
    joblib.dump(dummy_model, save_path)
    print(f"Training complete. Model saved to {save_path}")
```

Package the code into a tarball:

```
tar -czvf source.tar.gz train.py
```

Upload `source.tar.gz` to the S3 `code/` folder.

![source uploaded](/images/5-Workshop/5.2-Prerequisite/source-uploaded.png)

