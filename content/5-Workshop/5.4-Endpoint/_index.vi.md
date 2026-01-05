---
title : "Triển khai Serverless Endpoint"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Triển khai Serverless Endpoint

Trong phần này, chúng ta sẽ đóng gói Model Artifact được tạo ra từ bước Training và triển khai nó lên SageMaker Serverless Inference. Khác với Real-time Endpoint (chạy máy chủ 24/7), Serverless Endpoint sẽ tự động co giãn về 0 khi không sử dụng, giúp tối ưu chi phí cho các ứng dụng có lưu lượng truy cập không thường xuyên.
    
![ndpoint architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)


#### Nội dung

- [Tạo Model](4.1-model-create/)
- [Cấu hình Serverless Endpoint](4.2-serverless-endpoint/)
- [Deploy và Invoke](4.3-test-training/)
