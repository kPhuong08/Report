---
title : "Chuẩn bị tài nguyên"
date: 2026-01-05
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Sau khi Training Job ở phần 5.3 hoàn tất, file model.tar.gz sẽ xuất hiện trong S3 bucket folder output. 
![job](/images/5-Workshop/5.4-Endpoint/job-completed.png)

Sau một lúc thì job chạy xong

![job](/images/5-Workshop/5.4-Endpoint/ouput-model.png)

Tuy nhiên, file này chỉ là "xác" model. Để nó có thể "sống" và trả lời các câu hỏi (inference), chúng ta cần deploy nó lên một môi trường tính toán.

Trong bài lab này, chúng ta chọn SageMaker Serverless Inference.
+ Real-time Endpoint (Truyền thống): nếu thuê một con server chạy 24/7. Dù không có ai gọi vào, bạn vẫn mất tiền
+ Serverless Endpoint: AWS không giữ server nào chạy liên tục. Khi có request, AWS mới bật máy lên, load model, trả lời, rồi tắt máy. Bạn chỉ trả tiền cho vài giây xử lý đó.

#### Tạo model object

Bước này giống như việc đóng gói hành lý. SageMaker cần biết chính xác vị trí của model.tar.gz và source.tar.gz (code xử lý) để nạp vào Container.

1. Ở Sagemaker AI console bên thanh điều hướng chọn **Deployment & inference** 
2. Ở mục **Deployable model** click **Create model**

![model](/images/5-Workshop/5.4-Endpoint/create-model.png)

+ Model name: model-serverless
+ IAM role: role đã chuẩn bị ở phần trước

![model](/images/5-Workshop/5.4-Endpoint/info-model.png)

+ Container definition 1:
  + Container input options: Provide model artifacts and inference image location
  + Location of inference code image: 683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3 (Docker Image chứa môi trường Scikit-learn/Python.)
  + Location of model artifacts: link model output (s3://sagemaker-bucket-serverless/output/serverless-mlops-1767572462/output/model.tar.gz)

![model](/images/5-Workshop/5.4-Endpoint/info-container.png)

  + Environment variables: 2 cặp Key-Value để chỉ định file code chạy
    + SAGEMAKER_PROGRAM: train.py
    + SAGEMAKER_SUBMIT_DIRECTORY: link đến file code source.tar.gz

![model](/images/5-Workshop/5.4-Endpoint/info-env.png)

+ Sau khi config xong thì nhấn **Create model**

![model](/images/5-Workshop/5.4-Endpoint/model-created.png)





