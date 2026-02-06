# AWS Scalable Web Application 🚀

##  Project Overview
This project demonstrates how to design and deploy a **secure, highly available, and scalable web application** using AWS cloud services.  
It showcases real-world architecture with **EC2 instances, Application Load Balancer (ALB), Auto Scaling Group, VPC networking, and CloudWatch monitoring**.

---

##  Services Used
- **Amazon EC2** → Hosts the web application
- **Application Load Balancer (ALB)** → Distributes traffic across instances
- **Auto Scaling Group (ASG)** → Automatically adds/removes EC2 instances based on demand
- **Amazon VPC & Subnets** → Provides secure networking (public for ALB, private for EC2)
- **Security Groups** → Controls inbound/outbound traffic
- **CloudWatch** → Monitors CPU and triggers scaling policies

---

##  Architecture
- Website runs on multiple EC2 instances in **private subnets**
- ALB in **public subnets** routes traffic to healthy EC2s
- Auto Scaling ensures elasticity (min: 1, max: 4 instances)
- CloudWatch alarms trigger scaling events
- Bastion Host or SSM Session Manager used for secure access to private EC2s

![Architecture Diagram](architecture-diagram.png)

---

##  Setup Instructions
1. **Create VPC & Subnets**
   - 2 public subnets (for ALB)
   - 2 private subnets (for EC2)
   - Internet Gateway + Route Tables

2. **Configure Security Groups**
   - ALB SG → Allow HTTP/HTTPS from internet
   - EC2 SG → Allow HTTP from ALB SG

3. **Launch Template**
   - AMI: Amazon Linux 2
   - User Data script:
     ```bash
     #!/bin/bash
     yum update -y
     yum install -y httpd
     systemctl start httpd
     systemctl enable httpd
     echo "<h1>Hello from $(hostname)</h1>" > /var/www/html/index.html
     ```

4. **Create Target Group**
   - Protocol: HTTP
   - Port: 80
   - Health check path: `/index.html`

5. **Create Application Load Balancer**
   - Internet-facing
   - Listener: HTTP (80)
   - Forward traffic to target group

6. **Create Auto Scaling Group**
   - Attach Launch Template
   - Desired capacity: 2
   - Min: 1, Max: 4
   - Scaling policy: CPU > 70% → scale out, CPU < 30% → scale in

7. **Test**
   - Open ALB DNS in browser:
     ```
     web-app-ALB-1630821068.ap-south-1.elb.amazonaws.com
     ```
   - You should see:
     ```html
     <h1>Hello from ip-10-0-2-216.ap-south-1.compute.internal</h1>
     ```

---

## Screenshots
- ✅ EC2 instances healthy in Target Group  [https://github.com/sukhmaygiri/aws-scalable-webapp/blob/e5f0e539dd0b39b079a609b14a2b4897189a946b/target-group-healthy-instance.png]
- ✅ ALB DNS serving application [https://github.com/sukhmaygiri/aws-scalable-webapp/blob/f187c22390b421c7f427fd1cc5e0866837357c09/alb-dns-output.png]
- ✅ Auto Scaling adding/removing instances[]
- ✅ Debugging 502 errors resolved[]

---

##  Resume Snippet
*“Designed and deployed a secure, highly available web application using AWS EC2, Application Load Balancer, and Auto Scaling with CloudWatch-based scaling policies. Implemented health check diagnostics and Bastion Host access to ensure reliability and production-grade architecture.”*

---

## Key Learning Outcomes
- Hands-on with AWS networking (VPC, subnets, route tables)
- Configured secure access patterns (public ALB, private EC2, Bastion Host)
- Automated scaling with CloudWatch alarms
- Debugged real-world issues (502 Bad Gateway, health check failures)

---

##  Future Improvements
- Add HTTPS with AWS Certificate Manager
- Integrate AWS WAF for security
- Deploy a real application (Node.js, Python Flask, etc.) instead of static HTML
---

##  Future Improvements
- Add HTTPS with AWS Certificate Manager
- Integrate AWS WAF for security
- Deploy a real application (Node.js, Python Flask, etc.) instead of static HTML
