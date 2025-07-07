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

### 5. IAM Role: `Kishan-Project5`
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

- Service

- Purpose

- EC2

- Runs the Node.js-based e-commerce app

- Launch Template

- Template to launch EC2 instances with pre-configured settings

- Auto Scaling Group

- Automatically scales instances up/down based on CPU usage

- Application Load Balancer (ALB)

- Distributes traffic across instances in multiple AZs

- Target Group

- Registers EC2 instances behind the ALB

- CloudWatch Agent

- Collects system-level metrics like CPU, memory, and disk

- CloudWatch Alarms

- Triggers actions based on performance thresholds (e.g., CPU > 70%)

- SNS

- Sends email alerts when alarms are triggered

- IAM Role

- Grants EC2 permissions to write logs and send alerts

---
---

## User Data give in Both the EC2 instances
```bash
#!/bin/bash
sudo yum update -y
sudo yum -y install httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
sudo yum install mariadb-server
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
sudo mysql_secure_installation
sudo mysql -u root -p
sudo amazon-linux-extras enable php7.4
sudo yum install -y php php-cli php-mysqlnd php-fpm
sudo systemctl restart httpd
sudo systemctl enable httpd
sudo yum install  https://rpms.remirepo.net/enterprise/remi-release-7.rpm -y
sudo yum makecache
sudo yum install yum-utils
sudo  amazon-linux-extras | grep php
sudo yum-config-manager –disable ‘remi-php*’
sudo amazon-linux-extras enable php7.4
sudo yum clean
sudo yum install php-{curl,pear,gd,mbstring,bcmath,pdo,mbstring,mysqlnd,json,xml,zip,common,cgi,intl}
sudo yum install git -y
sudo git clone https://github.com/Ai-TechNov/ecomm.git
cd ecomm/
sudo cp ./index.html /var/www/html/
sudo systemctl restart httpd

```
---

---
## Configuring Cloud Watch Agent Via CLI
```bash
sudo yum update -y
sudo yum install -y amazon-cloudwatch-agent
```
# Create the directory structure (if it doesn't exist)
```bash
sudo mkdir -p /opt/aws/amazon-cloudwatch-agent/etc/
```
# Create/edit the configuration file using nano:
```bash
sudo nano /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
```

# Paste the configuration (use right-click or Shift+Insert to paste in most terminals):
```bash
{
    "agent": {
        "metrics_collection_interval": 60,
        "run_as_user": "root",
        "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log"
    },
    "metrics": {
        "namespace": "CWAgent",
        "metrics_collected": {
            "cpu": {
                "measurement": [
                    "cpu_usage_idle",
                    "cpu_usage_user",
                    "cpu_usage_system"
                ],
                "metrics_collection_interval": 60,
                "totalcpu": false
            },
            "disk": {
                "measurement": [
                    "used_percent",
                    "inodes_used"
                ],
                "metrics_collection_interval": 60,
                "resources": [
                    "/",
                    "/tmp"
                ]
            },
            "mem": {
                "measurement": [
                    "mem_used_percent",
                    "mem_available"
                ],
                "metrics_collection_interval": 60
            },
            "swap": {
                "measurement": [
                    "swap_used_percent"
                ],
                "metrics_collection_interval": 60
            }
        },
        "append_dimensions": {
            "InstanceId": "${aws:InstanceId}",
            "ImageId": "${aws:ImageId}",
            "InstanceType": "${aws:InstanceType}"
        }
    },
    "logs": {
        "logs_collected": {
            "files": {
                "collect_list": [
                    {
                        "file_path": "/var/log/messages",
                        "log_group_name": "/var/log/messages",
                         "log_stream_name": "{instance_id}"
                    }
                ]
            }
        }
    }
}

```

# Press Ctrl+O, then Enter to save, then Ctrl+X to exit

# Check the file was created properly:
sudo ls -la /opt/aws/amazon-cloudwatch-agent/etc/
# Validate the JSON syntax:
sudo cat /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json | jq empty
# (If you get "command not found", install jq with sudo yum install jq -y)
# Start the agent with configuration
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
    -a fetch-config \
    -m ec2 \
    -s \
    -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
```

# Enable service to start on boot
```bash
sudo systemctl enable amazon-cloudwatch-agent
```

# Verify agent status
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```
Creating CloudWatch Alarms via AWS Console for Amazon Linux 2

1. CPU Utilization > 70% Alarm
- Open CloudWatch Console:

- Navigate to https://console.aws.amazon.com/cloudwatch/

- Click "Alarms" in the left sidebar

- Click "Create alarm"

- Select Metric:

- Click "Select metric"

- Choose "CWAgent" namespace

- Select "InstanceId, cpu_usage_user" metric

- Select your instance's metric and click "Select metric"

- Configure Conditions:

- Set "Statistic" to "Average"

- Set "Period" to "5 minutes"

- Under "Conditions":

- Choose "Greater" > "70" > "percent"

"Additional configuration": 2 out of 2 datapoints

- Configure Actions:

- Select "Create new SNS topic" if you need notifications

- Enter a topic name and email recipients

Or select existing notification options

Add Details:

- Alarm name: High-CPU-Utilization-<YourInstanceName>

- Description: "Alarm when CPU exceeds 70% for 10 minutes"

- Click "Next"

- Preview and Create:

- Review settings

- Click "Create alarm"

2. Memory Usage > 80% Alarm
- Create new alarm (same initial steps as above)

Select Metric:

- Choose "CWAgent" namespace

- Select "InstanceId, mem_used_percent" metric

Select your instance's metric

Configure Conditions:

- Statistic: "Average"
- Period: "5 minutes"

Conditions:

- "Greater" > "80" > "percent"

2 out of 2 datapoints

- Configure Actions (same as CPU alarm)

Add Details:

- Alarm name: High-Memory-Usage-<YourInstanceName>

- Description: "Alarm when memory exceeds 80% for 10 minutes"

- Create alarm

3. Instance Status Check Failure Alarm

- Create new alarm

- Select Metric:

- Choose "AWS/EC2" namespace

- Select "Per-Instance Metrics"

- Select "StatusCheckFailed" metric

- Select your instance's metric

- Configure Conditions:

- Statistic: "Maximum"

- Period: "1 minute"

Conditions:

- "Greater/Equal" > "1"

- 1 out of 1 datapoints

- Configure Actions (same as previous alarms)

Add Details:

- Alarm name: Status-Check-Failed-<YourInstanceName>

- Description: "Alarm when instance status check fails"

Create alarm

Verification Steps
Check all alarms are created:

- Go to CloudWatch > Alarms

- You should see your three alarms in "Alarm state" (may take a few minutes to initialize)

Test alarms (optional):

- For CPU: Run a stress test (sudo yum install stress -y then stress --cpu 2 --timeout 300)

- For Status Check: Stop your instance (only do this in a test environment)

Important Notes
SNS Notifications:

- You'll need to confirm email subscriptions if you created new SNS topics

- Check your email for confirmation requests

Alarm States:

- Alarms may take 5-15 minutes to show initial data

- Green = OK, Red = Alarm, Gray = Insufficient data

Modifying Alarms:

- You can always edit alarms later by selecting them and clicking "Actions" > "Modify"

---
---
             
## 📬 Author

**Kishan Gollamudi**  
Trainee Software Engineer | AWS Cloud Practitioner  
Location: India  
Email: gnmskishan@gmail.com  
LinkedIn: https://www.linkedin.com/in/gollamudi-kishan2498/

---
