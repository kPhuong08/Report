---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Tự động hóa quy trình MLOps trên AWS sử dụng SageMaker theo hướng Serverless

#### Tổng quan

**Serverless MLOps**phương pháp tiếp cận hiện đại giúp giảm thiểu gánh nặng quản lý hạ tầng và tối ưu hóa chi phí bằng cách sử dụng các dịch vụ tự động co giãn của AWS.

Trong workshop này, chúng ta sẽ thực hành xây dựng một quy trình khép kín: từ lúc dữ liệu thô được đưa lên Cloud cho đến khi Model sẵn sàng phục vụ người dùng, tất cả đều được kích hoạt tự động mà không cần duy trì bất kỳ máy chủ (server) nào chạy liên tục.

Chúng ta sẽ tập trung vào các thành phần Serverless chính sau đây:
+ **SageMaker Serverless Inference** - Tính năng quan trọng nhất của bài lab. Cho phép triển khai Machine Learning Model dưới dạng API endpoint, tự động scale về 0 khi không có request. Bạn chỉ trả tiền cho thời gian tính toán thực tế.
+ **AWS Step Functions** - Đóng vai trò bộ điều phối (Orchestrator). Thay thế cho các server Jenkins/GitLab Runner truyền thống để quản lý luồng training và deployment.
+ **Amazon EventBridge** - Cơ chế kích hoạt theo sự kiện (Event-driven). Tự động phát hiện dữ liệu mới trên S3 để bắt đầu quy trình huấn luyện lại (Retrain) model.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Xây dựng luồng training tự động](5.3-Training-flow/)
4. [Triển khai Serverless Endpoint](5.4-Endpoint/)
5. [Dọn dẹp tài nguyên](5.5-Cleanup/)