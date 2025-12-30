---
title: "Bản đề xuất"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Automate MLOps 
## Tự động hóa quy trình Fine-tune và Deployment model trên AWS theo hướng Serverless   

### 1. Tóm tắt điều hành  
Serverless MLOps Pipeline được thiết kế nhằm giải quyết bài toán tối ưu hóa chi phí và tự động hóa quy trình huấn luyện (fine-tune) cũng như triển khai (deploy) mô hình máy học. Hệ thống tập trung vào việc loại bỏ các thao tác thủ công của kỹ sư dữ liệu, chuyển đổi từ mô hình quản lý hạ tầng truyền thống sang kiến trúc Event-driven và Serverless trên AWS.

Quy trình MLOps này cho phép tự động kích hoạt việc huấn luyện lại (re-training) khi có dữ liệu mới trên S3, thực hiện đánh giá (evaluation) tự động và triển khai mô hình lên SageMaker Serverless Inference. Giải pháp giúp giảm thiểu chi phí vận hành xuống mức thấp nhất nhờ tận dụng EC2 Spot Instances cho quá trình training và cơ chế auto-scaling về 0 của Serverless Endpoint cho quá trình inference.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Quy trình phát triển và vận hành mô hình ML hiện tại gặp nhiều trở ngại:
- Thủ công và rời rạc: Kỹ sư phải chạy lệnh thủ công để train, evaluate và deploy. Code base khó bảo trì.
- Lãng phí tài nguyên: Sử dụng các instance on-demand đắt đỏ và thường xuyên quên tắt sau khi train xong. Endpoint chạy liên tục 24/7 dù không có request.
- Thiếu kiểm soát chất lượng: Logic đánh giá (evaluation) sơ sài hoặc là mock (giả lập), dẫn đến rủi ro khi deploy model kém chất lượng lên production.

*Giải pháp*  
Xây dựng một pipeline CI/CD/CT (Continuous Training) hoàn chỉnh:
- Training: Sử dụng SageMaker Training Jobs với Spot Instances để giảm chi phí huấn luyện (tiết kiệm 50-80%).
- Evaluation: Tích hợp script đánh giá thật, so sánh metrics (Accuracy, F1) với threshold trước khi chấp nhận model.
- Inference: Chuyển sang SageMaker Serverless Endpoints, chỉ tính phí khi có request và miễn phí khi nhàn rỗi.
- Automation: Sử dụng Amazon EventBridge và GitHub Actions để điều phối quy trình từ lúc có dữ liệu mới đến khi deploy xong.

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Giải pháp mang lại hiệu quả kinh tế rõ rệt và giải phóng sức lao động cho kỹ sư AI/ML. Chi phí vận hành ước tính chỉ khoảng $3.30/tháng cho một kịch bản tiêu chuẩn (so với hàng chục USD nếu dùng hạ tầng tĩnh). Thời gian từ lúc có dữ liệu đến lúc model được update giảm từ vài ngày xuống vài giờ hoàn toàn tự động. Hệ thống CloudWatch Monitoring giúp phát hiện sự cố training hoặc inference ngay lập tức.  

### 3. Kiến trúc giải pháp  
Nền tảng áp dụng kiến trúc Event-driven Serverless để quản lý vòng đời của mô hình Machine Learning. Dữ liệu huấn luyện (CSV/JSON) được upload lên S3 sẽ kích hoạt chuỗi sự kiện tự động, từ training, đánh giá đến deploy.

![IoT Weather Station Architecture](/images/2-Proposal/edge_architecture.jpeg)

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

*Dịch vụ AWS sử dụng*  
- Amazon S3: Lưu trữ Datasets, Model Artifacts (model.tar.gz) và Training Metrics.
- Amazon SageMaker:
  + Training Jobs: Chạy Fine-tuning với HuggingFace Trainer (dùng Spot Instances).
  +  Serverless Inference: Hosting model endpoint với khả năng auto-scale (cold start 1-3s).
- AWS Lambda: Hàm serverless thực hiện logic Redeploy endpoint khi có model mới.
- Amazon EventBridge: Bắt sự kiện thay đổi trạng thái (S3 upload, Training success) để điều phối luồng.
- Amazon CloudWatch: Dashboard giám sát training metrics, endpoint latency và error rates. 

*Luồng dữ liệu*  
- Ingest: Người dùng upload dataset đã chuẩn bị lên Amazon S3.
- Trigger: S3 Event kích hoạt quy trình (qua EventBridge hoặc Webhook tới GitHub Actions).
- Train: GitHub Actions khởi tạo SageMaker Training Job (sử dụng HuggingFace container trên Spot Instance).
- Evaluate: Sau khi train xong, pipeline chạy script tải model về, kiểm tra metrics. Nếu đạt ngưỡng (threshold), model được lưu làm candidate.
- Deploy: Sự kiện có model mới kích hoạt AWS Lambda, hàm này sẽ cập nhật SageMaker Serverless Endpoint.

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án được chia thành các task cụ thể để đảm bảo tính chính xác và dễ debug:  
1. *Nghiên cứu và thử nghiệm*: Nghiên cứu các dịch vụ liên quan và thử nghiệm thủ công trên console.  
2. *Tính toán chi phí và kiểm tra tính khả thi*: Tính toán chi phí và các hỗ trợ của các dịch vụ xem có đáp ứng được mong muốn đề ra hay không.  
3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Tinh chỉnh (model, dịch vụ khác,...) để đảm bảo hiệu quả 
4. *Phát triển, kiểm thử, triển khai*: Cấu hình, viết script, tạo cơ sở hạ tầng
  - Clean & Refactor: Dọn dẹp code cũ, chuẩn hóa cấu trúc thư mục (src, ops, lambda).
  - Data Preparation Script: Viết script prepare_data.py để split train/test và upload lên S3.
  - Training Implementation: Cấu hình train.py sử dụng HuggingFace Trainer, tích hợp lưu metrics.json.
  - Evaluation Logic: Viết evaluate_model.py để parse metrics và quyết định Pass/Fail dựa trên threshold.
  - CI/CD Integration: Cấu hình GitHub Actions workflow để kết nối các bước trên.
  - Serverless Deployment: Viết Lambda function redeploy_endpoint sử dụng Boto3 để update SageMaker Endpoint.

Infrastructure as Code: Dùng Terraform để khởi tạo các tài nguyên AWS (S3, IAM Roles, Lambda). 

*Yêu cầu kỹ thuật*  
- Model: Các model HuggingFace (ví dụ: DistilBERT) cho tác vụ NLP.
- Compute: Training dùng ml.g4dn.xlarge (Spot); Inference dùng Serverless memory config 1024MB-3072MB.
- Latency: Chấp nhận Cold start 1-3s cho request đầu tiên sau thời gian nghỉ.

### 5. Lộ trình & Mốc triển khai  
- Tháng 1: Học AWS, tìm hiểu các dịch vụ, nghiên cứu hướng triển khai
- Tháng 2: Thử nghiệm và điều chỉnh
- Tháng 3: Triển khai, kiểm thử
- *Sau triển khai*: Nghiên cứu các hướng có thể phát triển   

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=d233d205f0008133dda74d0a8ba715346e5cc658)    

*Chi phí hạ tầng*  
- Training (SageMaker Spot):**~$3/tháng**.
- Inference (SageMaker Serverless): **$3/tháng**.
- Lưu trữ (Amazon S3): **~$0.3/tháng.**
- CloudWatch: **~$5/tháng**
- Orchestration (Lambda + EventBridge): **~$0.01/tháng** (gần như miễn phí trong Free Tier).

*Tổng*: ~$11 USD / tháng.

*(Lưu ý: Chi phí chưa bao gồm thuế và có thể biến động nhẹ tùy thuộc vào giá Spot Instance tại thời điểm chạy).*
 

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Spot Instance Interruption: Training bị ngắt giữa chừng do AWS thu hồi instance.
  - Giảm thiểu: Sử dụng Checkpointing trong code training để resume từ điểm dừng thay vì chạy lại từ đầu.
- Cold Start Latency: Request đầu tiên mất 1-3 giây, ảnh hưởng trải nghiệm người dùng real-time khắt khe.
  - Giảm thiểu: Chấp nhận được với các tác vụ nội bộ hoặc không yêu cầu realtime cực cao (sub-millisecond). Có thể dùng Provisioned Concurrency nếu cần (nhưng tốn phí hơn).
- Model Drift: Model mới train có performance kém hơn model cũ.
  - Giảm thiểu: Script evaluate_model.py chặn không cho deploy nếu metrics thấp hơn phiên bản hiện tại.

### 8. Kết quả kỳ vọng  
- Hệ thống tự động hóa 100%: Từ lúc có data đến lúc model lên sóng không cần can thiệp thủ công.
- Tối ưu chi phí: Giảm chi phí hạ tầng xuống mức thấp nhất có thể cho một dự án ML.
- Khả năng mở rộng: Dễ dàng áp dụng kiến trúc này cho các bài toán ML khác (Computer Vision, Tabular data) chỉ bằng cách thay đổi training script.
- Monitoring đầy đủ: Dashboard trực quan về sức khỏe của hệ thống training và serving.