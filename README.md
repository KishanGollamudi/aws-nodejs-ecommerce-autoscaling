# 🛍️ AWS Auto Scaling and Load Balancing for Node.js E-Commerce App

## 📌 Overview

A hands-on AWS DevOps project that demonstrates deploying a Node.js e-commerce application using EC2 instances, Application Load Balancer (ALB), Auto Scaling Group (ASG), IAM roles, and CloudWatch Alarms with SNS email notifications. The project ensures high availability, fault tolerance, and monitoring.

It ensures high availability, fault tolerance, and automated monitoring and alerting.

---

## 🛠️ Technologies & Services Used

- **Amazon EC2** – for hosting web servers
- **Elastic Load Balancer (ALB)** – to distribute traffic
- **Auto Scaling Group (ASG)** – to handle dynamic instance scaling
- **Amazon CloudWatch** – for monitoring and alerting
- **Amazon SNS** – for email notifications
- **IAM Roles** – for EC2 permissions
- **Launch Template** – for launching new EC2 instances
- **Amazon Linux AMI** – as base image for EC2
- **Node.js** – E-Commerce App backend

---

## 🌐 Architecture Overview

![Image](https://github.com/user-attachments/assets/e0fc40c4-1c32-4c1d-aa37-23984aa99271)
              

---

## ⚙️ Setup Breakdown

### 1. Load Balancer: `ABL-Kishan-project5`
- Type: **Application**
- Scheme: **Internet-facing**
- Status: ✅ *Active*
- Hosted in **2 AZs** (us-east-1a, us-east-1b)
- DNS: `ABL-Kishan-project5-213808085.us-east-1.elb.amazonaws.com`

### 2. Target Group: `kishan-project5`
- Target type: **Instance**
- Protocol: **HTTP (port 80)**
- Total targets: 4
  - ✅ 1 Healthy
  - ❌ 3 Unhealthy

### 3. Auto Scaling Group: `kishan-project5-AS`
- Launch Template: `kishan-project5`
- AMI: `ami-000ec6c25978d5999`
- Instance Type: `t2.micro`
- Capacity: min=2, max=4, desired=2
- Zones: **us-east-1a & us-east-1b**

### 4. Launch Template
- Name: `kishan-project5`
- Version: `v0.1 (Default)`
- Security Groups:
  - `sg-03cd7f12cd91ee1f0`
  - `sg-05440c9f9581e351d`
  - `sg-077a0750357b2b282`
  - `sg-0e523fb5ee345e569`
- Key Pair: `project`

### 5. IAM Role: `Aja-Kishan-Project5`
- Policies attached:
  - `AmazonEC2RoleforSSM`
  - `AmazonSNSFullAccess`
  - `CloudWatchAgentServerPolicy`

### 6. EC2 Instances
- `Web-app-1`, `Web-app-2`, etc.
- All instances: ✅ *Running*
- Instance types: `t2.micro`, `t3.micro`
- IAM Role attached: `Aja-Kishan-Project5`

### 7. CloudWatch Alarms
| Alarm Name                             | Status     | Condition                             |
|----------------------------------------|------------|----------------------------------------|
| Status check failed <Web-app-1>        | ⏸️ Insufficient Data | StatusCheckFailed_Instance >= 1         |
| high-memory-usage <Web-app-1>          | ✅ OK       | mem_used_percent > 80 (10 mins)        |
| high-cpu-utilization <Web-app-1>       | ✅ OK       | disk_used_percent > 70 (10 mins)       |
| TargetTracking-AS-AlarmLow             | ❌ In Alarm | CPUUtilization < 49 for 15 datapoints  |
| TargetTracking-AS-AlarmHigh            | ✅ OK       | CPUUtilization > 70 for 3 datapoints   |

### 8. SNS Topic Subscription
- Topic ARN: `arn:aws:sns:us-east-1:471112556180:kishan-project`
- Confirmed Email: ✅ `Gollamudi Naga Maruthi Srinivas Kishan`

---

## 📸 Screenshots Included in `/images`

- Load Balancer Configuration
- Target Group Health
- Auto Scaling Group Settings
- IAM Role and Policies
- EC2 Instance Details
- Launch Template
- CloudWatch Alarms
- Email Confirmation for SNS

---
---

## 🧩 Main AWS Components Used

Service

Purpose

EC2

Runs the Node.js-based e-commerce app

Launch Template

Template to launch EC2 instances with pre-configured settings

Auto Scaling Group

Automatically scales instances up/down based on CPU usage

Application Load Balancer (ALB)

Distributes traffic across instances in multiple AZs

Target Group

Registers EC2 instances behind the ALB

CloudWatch Agent

Collects system-level metrics like CPU, memory, and disk

CloudWatch Alarms

Triggers actions based on performance thresholds (e.g., CPU > 70%)

SNS

Sends email alerts when alarms are triggered

IAM Role

Grants EC2 permissions to write logs and send alerts


---

## 📬 Author

**Kishan Gollamudi**  
Trainee Software Engineer | AWS Cloud Practitioner  
Location: India  
Email: gnmskishan@gmail.com  
LinkedIn: https://www.linkedin.com/in/gollamudi-kishan2498/

---
