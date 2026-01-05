---
title : "Xây dựng luồng Training tự động"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Xây dựng luồng Training tự động

Trong phần này, chúng ta sẽ thiết lập cơ chế Event-Driven (hướng sự kiện). Mục tiêu là khi dữ liệu training mới được tải lên S3 Bucket, một Lambda Function sẽ tự động được kích hoạt. Lambda này đóng vai trò là "Orchestrator" để gọi API của SageMaker, bắt đầu một Training Job mới mà không cần sự can thiệp của con người.

![overview](/images/5-Workshop/5.3-Training-flow/diagram2.png)

#### Nội dung

- [Tạo Lambda function](3.1-lambda-function/)
- [Cấu hình S3 Event & kiểm thử](3.2-s3-event-trigger/)