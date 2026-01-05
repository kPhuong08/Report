---
title : "Giới thiệu"
date: 2026-01-05
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tự động hóa quy trình MLOps trên AWS sử dụng SageMaker theo hướng Serverless

+ **MLOps** 
Là sự kết hợp giữa Machine Learning, DevOps và Data Engineering. Là một quy trình tự động hóa khép kín: từ lúc lấy dữ liệu (Data Ingestion), huấn luyện (Training), đánh giá (Evaluation) cho đến triển khai (Deployment) và giám sát (Monitoring). Mục tiêu là giảm thiểu thao tác thủ công và tăng tốc độ đưa model ra thị trường.
+ **SageMaker AI**
Là nền tảng toàn diện của AWS giúp xây dựng, huấn luyện và triển khai các mô hình ML. Trong bài lab này, chúng ta sẽ tận dụng các tính năng hiện đại nhất của SageMaker:
  + **SageMaker Training:** Sử dụng Spot Instances để tiết kiệm chi phí huấn luyện.
  + **SageMaker Serverless Inference:** Triển khai model API mà không cần quản lý server, tự động scale về 0 khi không sử dụng (Cost Optimization).
  + **SageMaker Pipelines (hoặc Step Functions):** Để điều phối luồng công việc.


![overview](/images/5-Workshop/5.1-Workshop-overview/arch.png)
