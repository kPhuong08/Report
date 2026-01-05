---
title : "Kiểm tra Gateway Endpoint"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Cấu hình S3 Event Trigger

1. Tại Lambda function 
2. Chọn **Add trigger**　với source là **S3** 

![add trigger](/images/5-Workshop/5.3-S3-vpc/add-trigger.png)

![trigger source](/images/5-Workshop/5.3-S3-vpc/trigger-source.png)

3. Trong Trigger configuration 
+ Chọn bucket: là bucket đã tạo 
+ Event typed: PUT
+ Prefix: `data/train/` (Chỉ kích hoạt khi upload vào folder này)
+ Suffix: .csv (Chỉ kích hoạt với file csv)

![trigger config](/images/5-Workshop/5.3-S3-vpc/trigger-config.png)

Sau đó tick vào "I acknowledge..." và nhấn **Add**

![trigger added](/images/5-Workshop/5.3-S3-vpc/trigger-added.png)

### Kiểm thử luồng 

1. Tạo một file data test-train.csv
2. Upload file lên S3 path `/data/train/`

Di chuyển đến S3, chọn bucket vừa tạo, vào folder `/data/train/`

Click **Upload** 

![s3](/images/5-Workshop/5.3-S3-vpc/choose-upload.png)

Nhấn **Add files** chọn file data .csv đã chuẩn bị sau đó **Upload** lên S3

![s3](/images/5-Workshop/5.3-S3-vpc/add-files.png)

Sau khi upload thành công 

![s3](/images/5-Workshop/5.3-S3-vpc/uploaded.png)

3. Di chuyển đến Sagemaker AI vào mục **Model training & customization** chọn **Training & tuning jobs** sẽ thấy một Job mới đang ở trạng thái InProgress. Điều này chứng tỏ Event-Driven workflow đã hoạt động thành công.

![sagemaker](/images/5-Workshop/5.3-S3-vpc/job-created.png)

