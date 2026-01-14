---
title: "Event 2"
date: 2026-01-05
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Report: “Secure Your Applications: AWS Perimeter Protection Workshop”

### Event Objectives

- Sharing insights on CloudFront, WAF, and AWS Shield.
- Introduction to designing and optimizing global content delivery with CloudFront.
- Methods to protect web applications from threats using AWS WAF.
- Hands-on practice on security and web application optimization.

### List of Speakers

- **Nguyen Gia Hung** - Head of Solutions Architect, AWS
- **Julian Ju** - Senior Edge Services Specialist Solutions Architect, AWS

### Key Highlights

#### Introduction to CloudFront

- **Operation**: Operates based on a network of Points of Presence (PoPs) to route traffic to the location closest to the user.
- **Connectivity & Protocols**: HTTP/3 Multiplexing, TCP Optimization (Persistent Connection, Connection Pooling, TCP Window Scaling), AWS Global Backbone.
- **Origin cloaking**: Hiding the origin server (S3 "block all" - best practice).
- **Access Control**: Signed URL/Cookies, Geographic Restriction.
- **Performance**: Multi-layer cache, request collapsing, TTL (Time-to-Live).
- **Reliability**: CloudFront Origin Failover (Request Level), DNS Failover (Route 53 Global Scale), Logic at Edge.
- **Observability**: CloudWatch RUM (Real User Monitoring), Internet Monitor.

#### Security Issues in Applications

- DDoS attacks on network infrastructure.
- Application vulnerabilities causing attack risks.
- Bot traffic (fake clients) depleting resources.
- Brute-force and flood attacks.

#### Introduction to WAF and Application Protection

- **AWS Shield**: Protects layers 3-4, automatically detects and blocks DDoS attacks on network infrastructure.
- **AWS WAF**: Protects layer 7, blocks application vulnerabilities (CVE), SQLi, XSS.
- **Bot Control**: Prevents bot traffic (fake clients) that causes cost overruns and data skew.
- **Rate limit**: Limits the number of requests to prevent brute-force or flood attacks.

### Lessons Learned

#### AWS CloudFront

- Understood how CloudFront works and explored enterprise case studies using CloudFront.
- Learned how to use CloudFront and how to optimize web applications with it.

#### Security with WAF

- Learned about security risks and vulnerabilities.
- Understood how WAF operates and how it prevents security attacks.
- Learned how to use WAF to prevent vulnerabilities and basic attacks.

### Application to Work

- **Applying CloudFront in Web Deployment**: Using CloudFront to optimize Web applications  and deploy in a Serverless direction, optimizing costs for simple websites that do not require heavy processing.
- **Applying WAF in Security**: Using WAF  to secure data flows within the system, allowing trusted data flows to pass while blocking suspicious ones.

### Event Experience

Participating in the **“Secure Your Applications: AWS Perimeter Protection Workshop”** was a very rewarding experience, helping me gain knowledge about CDNs, optimizing Web application deployment on the internet, and cloud security. Some standout experiences included:

#### Learning from High-Expertise Speakers
- AWS speakers shared **best practices** in design optimization and application security.
- Through real-world case studies, I clearly understood how to apply **CloudFront** and **AWS WAF** in practical scenarios.

#### Hands-on Technical Experience
- The workshop included practicing the deployment of a Web application on CloudFront, which helped me understand how CloudFront operates.
- Practicing security with WAF through various scenarios helped me understand how WAF secures applications and how to deploy security configurations based on specific needs.

#### Networking and Exchange
- Participating in listening and discussing with speakers as well as fellow attendees helped me connect and learn more about AWS services, specifically CloudFront and WAF.
- Practical implementation of CloudFront and WAF under the guidance of speakers helped me learn the methods and understand the mechanics of these services better.

#### Key Takeaways
- **CloudFront**: A CDN service that assists in optimizing Web application deployment on the Internet.
- **WAF**: A security service that helps users prevent security vulnerabilities as well as cyber attacks on applications.

> Overall, the event not only provided technical knowledge but also helped me learn about services that support efficient, cost-optimized, and more secure Web application deployment.