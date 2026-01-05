---
title : "Dọn dẹp tài nguyên"
date: 2026-01-05
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Dọn dẹp tài nguyên
Đây là bước quan trọng nhất để đảm bảo bạn không phát sinh chi phí AWS sau khi kết thúc Workshop. Chúng ta sẽ xóa các tài nguyên theo thứ tự ngược lại với quy trình tạo: Endpoint -> Model -> Data.

#### Xóa SageMaker Resources
*Thay tên endpoint của bạn vào đây*
1. Xóa Endpoint
```
aws sagemaker delete-endpoint --endpoint-name demo-serverless-endpoint
# Hoặc tên endpoint tạo bằng CLI
aws sagemaker delete-endpoint --endpoint-name demo-serverless-endpoint
```
2. Xóa Endpoint Config

```
aws sagemaker delete-endpoint-config --endpoint-config-name demo-serverless-config
# Xóa cả config console nếu có
aws sagemaker delete-endpoint-config --endpoint-config-name demo-serverless-config
```
3. Xóa model

```
# Lấy danh sách model để copy tên cho chính xác
aws sagemaker list-models

# Xóa model
aws sagemaker delete-model --model-name demo-serverless-model
```
### Xóa Dữ liệu và Lambda
1. Dọn sạch S3 Bucket
```
# Thay tên bucket của bạn
export BUCKET_NAME="bucket của bạn"

# Xóa toàn bộ objects (bao gồm code, data, model artifact)
aws s3 rm s3://$BUCKET_NAME --recursive

# Xóa bucket
aws s3 rb s3://$BUCKET_NAME
```

2. Xóa Lambda Function
+ Vào Lambda console
+ Chọn function cần xóa
+ chọn **Action** -> **Delete**

![delete](/images/5-Workshop/5.5-Cleanup/delete-function.png)

3. Xóa CloudWatch Log Groups
+ Vào CloudWatch -> **Logs** -> **Log Management**
+ Chọn các log cần xóa -> **Action** -> **Delete log group(s)**

![delete](/images/5-Workshop/5.5-Cleanup/delete-log.png)
