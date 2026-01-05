---
title : "Các bước chuẩn bị"
date: 2026-01-05
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---
### Tạo S3 Bucket
Trước hết, chúng ta cần một nơi để lưu trữ dữ liệu huấn luyện và model artifacts.

![create bucket](/images/5-Workshop/5.2-Prerequisite/create-bucket.png)

![set name bucket](/images/5-Workshop/5.2-Prerequisite/name-bucket.png)

![enable versioning](/images/5-Workshop/5.2-Prerequisite/enable-versioning.png)

Các thông tin còn lại để default

Tạo thêm các folder rỗng để có thể upload file để trigger, chứa dữ liệu, lưu model và lưu script 
* `data/train/`: Nơi bạn sẽ upload file để trigger training.
* `data/test/`: Nơi chứa dữ liệu kiểm thử.
* `models/`: Nơi SageMaker lưu model sau khi train.
* `code/`: Nơi lưu script training (`train.py`)

![create folder](/images/5-Workshop/5.2-Prerequisite/create-folder.png)

![folders](/images/5-Workshop/5.2-Prerequisite/folders.png)

#### IAM permissions & role
Vì chúng ta sử dụng kiến trúc Serverless tự động, các dịch vụ cần quyền để "nói chuyện" với nhau. Vào IAM Console, tạo một Role tên là SageMakerExecutionRole với các policy:

* AmazonSageMakerFullAccess
* AmazonS3FullAccess
* CloudWatchLogsFullAccess 
* AWSLambda_FullAccess
*Lưu lại ARN của Role này để dùng ở các bước sau.*

Vào console của IAM, trong phần Role:

![create role](/images/5-Workshop/5.2-Prerequisite/create-role.png)

Nhấn **Create** để tạo role mới

![info role](/images/5-Workshop/5.2-Prerequisite/info-role.png)

Ở đây chúng ta sẽ tạo role cho service **Sagemaker** và nhấn **Next**

![role permission](/images/5-Workshop/5.2-Prerequisite/role-permission.png)

Thì permission default cho role khi tạo sẽ là **"AmazonSageMakerFullAccess"** và nhấn **Next**

![name role](/images/5-Workshop/5.2-Prerequisite/name-role.png)

Nhập tên và mô tả cho role còn lại để dault và chọn **Create role** ở cuối trang

![role created](/images/5-Workshop/5.2-Prerequisite/role-created.png)

Role đã được tạo. Sau đó tiến hành thêm policy cho role:

![add permission](/images/5-Workshop/5.2-Prerequisite/add-permission.png)

Nhấn vào role vừa tạo ở mục permission nhấn **Add permission** và chọn **Attach policies**

![choose permission](/images/5-Workshop/5.2-Prerequisite/choose-permission.png)

Search các role cần thiết và tick vào ô như hình trên (*các policy khác tương tự*)

![choose add](/images/5-Workshop/5.2-Prerequisite/choose-add.png)

Sau khi thêm những policy cần thiết thì nhấn **Add permission**

![permission added](/images/5-Workshop/5.2-Prerequisite/permission-added.png)

Permission mới đã được thêm vào 

#### Training Script & Data

Chúng ta cần chuẩn bị sẵn mã nguồn training và dữ liệu mẫu ở máy local để sẵn sàng cho việc upload.

Tùy vào model sẽ có những dataset khác nhau ở đây chúng ta tạo một lệnh train đơn giản để giả lập

Tạo file train.py ở máy local với nội dung giả lập quá trình training:
```
import argparse
import os
import time
import joblib  # <--- Nhớ import thư viện này

# ---------------------------------------------------------
# PHẦN 1: CÁC HÀM BẮT BUỘC CHO SAGE MAKER ENDPOINT (INFERENCE)
# ---------------------------------------------------------

def model_fn(model_dir):
    """
    Hàm này được SageMaker gọi TỰ ĐỘNG khi khởi động Endpoint.
    Nhiệm vụ: Load model từ ổ cứng lên RAM.
    """
    print(f"Đang load model từ: {model_dir}")
    model_path = os.path.join(model_dir, 'model.joblib')
    
    if os.path.exists(model_path):
        # Load model ra. Ở dưới mình lưu là Dictionary nên load ra cũng là Dictionary
        return joblib.load(model_path)
    else:
        raise FileNotFoundError(f"Không tìm thấy file model tại: {model_path}")

def predict_fn(input_data, model):
    """
    Hàm này nhận data từ user và đưa vào model
    """
    # Vì model là dummy (chỉ là cái Dict), trả về nguyên văn
    return {"status": "success", "prediction": model, "input_received": input_data}

# ---------------------------------------------------------
# PHẦN 2: CODE TRAINING 
# ---------------------------------------------------------

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    
    # SageMaker tự động mount data vào các đường dẫn này
    parser.add_argument('--train', type=str, default=os.environ.get('SM_CHANNEL_TRAIN'))
    parser.add_argument('--model-dir', type=str, default=os.environ.get('SM_MODEL_DIR'))
    
    args, _ = parser.parse_known_args()
    
    print("Starting training...")
    time.sleep(5) # Giả lập training
    
    # Dùng joblib.dump để lưu object thật
    # Để tí nữa hàm model_fn ở trên có thể load lại được bằng joblib.load
    dummy_model = {"name": "Dummy Model", "accuracy": 99.9}
    
    save_path = os.path.join(args.model_dir, 'model.joblib')
    joblib.dump(dummy_model, save_path)
    # ------------------------
        
    print(f"Training complete. Model saved to {save_path}")
```

Nén file này lại thành .tar.gz
```
tar -czvf source.tar.gz train.py
```
![source](/images/5-Workshop/5.2-Prerequisite/source.png)

Upload file source.tar.gz lên S3 folder code/

![source uploaded](/images/5-Workshop/5.2-Prerequisite/source-uploaded.png)