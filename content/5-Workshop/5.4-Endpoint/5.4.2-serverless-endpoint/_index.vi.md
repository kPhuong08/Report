---
title : "Cấu hình Serverless Endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

### Tạo Endpoint Configuration

1. Ở Sagemaker AI console bên thanh điều hướng chọn **Deployment & inference** 
2. Ở mục **Endpoint Configuration** click **Create endpoint configuration**

![endpoint](/images/5-Workshop/5.4-Endpoint/create-end-config.png)

+ Name: serverless-endpoint
+ Type of endpoint: **Serverless**

![endpoint](/images/5-Workshop/5.4-Endpoint/info-end-config.png)

+ Click **Create production variant**
+ Chọn model được tạo ở bước trên
+ Nhấn **Save**

![endpoint](/images/5-Workshop/5.4-Endpoint/production-variant.png)

![endpoint](/images/5-Workshop/5.4-Endpoint/production-created.png)

+ Kéo ngang sang phải để có thể Edit/remove Variant

![endpoint](/images/5-Workshop/5.4-Endpoint/edit-production.png)

  + Name: variant-serverless
  + Max concurrency: 10 (đây là số lượng default, nếu muốn thêm thì cần request lên AWS để mở rộng)

![endpoint](/images/5-Workshop/5.4-Endpoint/info-production.png)

+ Nhấn **Create endpoint configuration**

![endpoint](/images/5-Workshop/5.4-Endpoint/end-config-created.png)

### Tạo Endpoint

1. Ở Sagemaker AI console bên thanh điều hướng chọn **Deployment & inference** 
2. Ở mục **Endpoint** click **Create endpoint**

![endpoint](/images/5-Workshop/5.4-Endpoint/create-end.png)

+ Name: endpoint-deploy-serverless
+ Attach endpoint configuration: Use an existing endpoint configuration
+ Chọn endpoint configuration vừa tạo ở bước trên 
+ Nhấn **Select endpoint configuration**

![endpoint](/images/5-Workshop/5.4-Endpoint/info-end.png)

+ Nhấn **Create endpoint**

![endpoint](/images/5-Workshop/5.4-Endpoint/end-created.png)


