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
import shutil
import json 

# ---------------------------------------------------------
# PHẦN 1: CÁC HÀM INFERENCE 
# ---------------------------------------------------------

def model_fn(model_dir):
    """Load model từ ổ cứng"""
    print(f"Đang load model từ: {model_dir}")
    model_path = os.path.join(model_dir, 'model.joblib')
    
    if os.path.exists(model_path):
        return joblib.load(model_path)
    else:
        raise FileNotFoundError(f"Không tìm thấy file model tại: {model_path}")

def input_fn(request_body, request_content_type):
    """
    Hàm này nhận Raw Bytes từ request và chuyển thành object Python
    """
    if request_content_type == 'application/json':
        # Chuyển chuỗi JSON (bytes) thành Python List/Dict
        return json.loads(request_body)
    else:
        # Nếu gửi content-type khác, báo lỗi
        raise ValueError(f"Content type {request_content_type} chưa được hỗ trợ.")

def predict_fn(input_data, model):
    """
    Logic dự đoán
    """
    # Vì đây là dummy model (Dict), ta giả lập việc dự đoán
    # input_data lúc này đã là List/Dict nhờ hàm input_fn ở trên
    
    result = {
        "status": "success",
        "prediction_from_model": model, # Model dummy của bạn
        "input_you_sent": input_data
    }
    return result

def output_fn(prediction, content_type):
    """
    Hàm này nhận kết quả từ predict_fn và chuyển thành JSON để trả về Client.
    QUAN TRỌNG: Khắc phục lỗi 500 tại đây.
    """
    # Mặc định SageMaker mong đợi accept header là application/json
    if content_type == 'application/json':
        return json.dumps(prediction)
    
    raise ValueError(f"Accept type {content_type} chưa được hỗ trợ.")

# ---------------------------------------------------------
# PHẦN 2: CODE TRAINING
# ---------------------------------------------------------

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    
    parser.add_argument('--train', type=str, default=os.environ.get('SM_CHANNEL_TRAIN'))
    parser.add_argument('--model-dir', type=str, default=os.environ.get('SM_MODEL_DIR'))
    
    args, _ = parser.parse_known_args()
    
    print("Starting training...")
    # time.sleep(5) 
    
    dummy_model = {"name": "Dummy Model", "accuracy": 99.9}
    
    save_path = os.path.join(args.model_dir, 'model.joblib')
    joblib.dump(dummy_model, save_path)
        
    print(f"Training complete. Model saved to {save_path}")

    # Lấy đường dẫn file code đang chạy hiện tại
    current_script_path = __file__
    
    # Định nghĩa đường dẫn đích trong folder output
    code_output_path = os.path.join(args.model_dir, 'train.py')
    
    print(f"Copying code from {current_script_path} to {code_output_path}")
    shutil.copy2(current_script_path, code_output_path)
    
    # =========================================================

    print("Training and packaging complete.")
```

Package the code into a tarball:

```
tar -czvf source.tar.gz train.py
```

Upload `source.tar.gz` to the S3 `code/` folder.

![source uploaded](/images/5-Workshop/5.2-Prerequisite/source-uploaded.png)

