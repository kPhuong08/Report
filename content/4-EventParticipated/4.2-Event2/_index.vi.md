---
title: "Event 2"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “Secure Your Applications: AWS Perimeter Protection Workshop”

### Mục Đích Của Sự Kiện

- Chia sẻ CloudFront, WAF, AWS Shield
- Giới thiệu về cách thiết kế và tối ưu hóa việc phân phối nội dung toàn cầu với CloudFront
- Phương pháp bảo vệ các ứng dụng web khỏi các mối đe dọa bằng cách sử dụng AWS WAF 
- Thực hành bảo mật và tối ưu hóa ứng dụng Web 

### Danh Sách Diễn Giả

- **Nguyen Gia Hung** - Head of Solutions Architect, AWS
- **Julian Ju** - Senior Edge Services Specialist Solutions Architect, AWS

### Nội Dung Nổi Bật

#### Giới thiệu về CloudFront

- **Hoạt động**: hoạt động dựa trên mạng lưới các điểm hiện diện (PoPs) để định tuyến traffic đến nơi gần người dùng nhất.
- **Connectivity & Protocols**: HTTP/3 Multiplexing, TCP Optimization (Persistent Connection, Connection Pooling, TCP Window Scaling), AWS Global Backbone
- **Origin cloaking**: ẩn server gốc (S3 "block all" - best practice) 
- **Access Control**: Signed URL/Cookies, Geographic Restriction
- **Performance**: multi-layer cache, request collapsing, TTL (Time-to-Live)
- **Reliability**: CloudFront Origin Failover (Request Level), DNS Failover (Route 53 Global Scale), Logic tại Edge
- **Observability**: CloudWatch RUM (Real User Monitoring), nternet Monito

#### Các vấn đề về bảo mật trong ứng dụng 

- Các cuộc tấn công DDoS vào hạ tầng mạng 
- Các lỗ hổng ứng dụng gây nguy cơ bị tấn công
- Bot traffic (fake client) làm hao hụt tài nguyên
- Các cuộc tấn công brute-force và flood

#### Giới thiệu về WAF và cách bảo vệ ứng 

- **AWS Shield**: bảo vệ layer3-4, tự động phát hiện và chặn các cuộc tấn công DDoS vào hạ tầng mạng.
- **AWS WAF**: bảo vệ layer7, chặn các lỗ hổng ứng dụng (CVE), SQLi, XSS
- **Bot Control**: ngăn chặn bot traffic (fake client) gây tốn chi phí và sai lệch dữ liệu.
- **Rate limit**: giới hạn số lượng request để chống brute-force hoặc flood.


### Những Gì Học Được

#### AWS CloudFront

- Hiểu thêm về cách hoạt động của Cloudfront, các case-study trong doanh nghiệp với CloudFront
- Biết được cách sử dụng CloudFront và cách tối ưu ứng dụng web với CloudFront

#### Bảo mật với WAF

- Biết được các nguy cơ và lỗ hổng bảo mật
- Tìm hiểu được cách hoạt động của WAF và cách WAF ngăn chặn các cuộc tấn công bảo mật
- Biết cách sử dụng WAF để ngăn chặn các lỗ hổng và các cuộc tấn công cơ bản 

### Ứng Dụng Vào Công Việc

- **Áp dụng CloudFront trong triển khai web**: Sử dụng CloudFront cho việc tối ưu ứng dụng Web và triển khai theo hướng Serverless, tối ưu chi phí với những web đơn giản không cần xử lý nhiều
- **Áp dụng WAF trong bảo mật**: sử dụng WAF cho việc bảo mật luỗng dữ liệu trong hệ thống, với việc cho phép những luồng dữ liệu tin cậy đi qua và chặn nhựng luồng dữ liệu đáng ngờ 

### Trải nghiệm trong event

Tham gia workshop **“Secure Your Applications: AWS Perimeter Protection Workshop ”** là một trải nghiệm rất bổ ích, giúp tôi có thêm kiến thức về CDN, tối ưu việc triển khai ứng dụng Web trên internet và bảo mật trong cloud. Một số trải nghiệm nổi bật:

#### Học hỏi từ các diễn giả có chuyên môn cao
- Các diễn giả đến từ AWS đã chia sẻ **best practices** trong tối ưu thiết kế, bảo mật ứng dụng
- Qua các case study thực tế, tôi hiểu rõ hơn cách áp dụng **CloudFront** và **AWS WAF** vào các trường hợp thực tế.

#### Trải nghiệm kỹ thuật thực tế
- Workshop thực hành triển khai một ứng dụng Web trên CloudFront giúp tôi hiểu về cách CloudFront hoạt động 
- Thực hành bảo mật bằng WAF với nhiều kịch bản khác nhau giúp tôi hiểu về cách WAF bảo mật và triển khai bảo mật theo nhu cầu của mình trên WAF  


#### Kết nối và trao đổi
- Việc tham gia nghe và thảo luận với các diễn giả cũng như bạn đồng tham dự, giúp tôi kết nối và học hỏi thêm về những dịch vụ trên AWS cụ thể là CloudFront và WAF
- Việc thực hành triển khia CloudFront cũng như WAF dưới sự hướng dẫn của diễn giả giúp tôi biết thêm về cách thức cũng như hiểu thêm về cách hoạt động của những dịch vụ này

#### Bài học rút ra
- **CloudFront**: một dịch vụ CDN có thể giúp ích trong viêc tối ưu triển khai ứng dụng Web trên Internet 
- **WAF**: dịch vụ bảo mật giúp người dùng có thể ngăn chặn các lỗ hổng bảo mật cũng như các cuộc tấn công mạng cho ứng dụng

#### Một số hình ảnh khi tham gia sự kiện
* Thêm các hình ảnh của các bạn tại đây

> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi biết thêm về những dịch vụ hỗ trợ trong việc triển khai ứng Web một cách hiệu quả, tối ưu chi phí và bảo mật hơn.
