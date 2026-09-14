# Basic SNS Project — Real Use Case

## Project: EC2 CPU Alert → SNS → Email

**Use case:** A company wants the administrator to automatically receive an email when an EC2 instance's CPU utilization becomes high.

### Architecture

```text
                    AWS Cloud
                       
 ┌────────────┐
 │ EC2 Server │
 └─────┬──────┘
       │ CPUUtilization
       ▼
 ┌──────────────────┐
 │ CloudWatch Alarm │
 │    CPU > 70%     │
 └────────┬─────────┘
          │ Trigger
          ▼
 ┌──────────────────┐
 │    SNS Topic     │
 │ server-cpu-alert │
 └────────┬─────────┘
          │ Push Notification
          ▼
 ┌──────────────────┐
 │      Email       │
 │      Admin       │
 └──────────────────┘
```

## Scenario

```text
Company: Cloudnautic
Server: Production Web Server
Problem: Need notification when CPU becomes high
Condition: CPU > 70%
Action: Send email to administrator
```

## Step 1 — Launch EC2

Create a basic EC2 instance:

```text
Name: sns-webserver
AMI: Amazon Linux 2023
Instance Type: t3.micro
Security Group:
SSH  → 22
HTTP → 80
```

Install a web server:

```bash
sudo dnf install httpd -y
sudo systemctl enable --now httpd

echo "<h1>SNS Monitoring Web Server</h1>" | \
sudo tee /var/www/html/index.html
```

## Step 2 — Create SNS Topic

Go to:

**SNS → Topics → Create topic**

```text
Type: Standard
Name: server-cpu-alert
```

Create the topic.

## Step 3 — Add Email Subscriber

Open the topic → **Create subscription**.

```text
Protocol: Email
Endpoint: your-email@example.com
```

Check your inbox and click **Confirm subscription**.

```text
SNS Topic
    │
    └──────► Email Admin
```

## Step 4 — Create CloudWatch Alarm

Go to:

**CloudWatch → Alarms → Create alarm → Select metric**

Choose:

```text
EC2
  ↓
Per-Instance Metrics
  ↓
CPUUtilization
```

Configure:

```text
Metric: CPUUtilization
Statistic: Average
Period: 1 minute

Threshold:
CPUUtilization > 70%
```

Notification:

```text
Alarm State → SNS Topic → server-cpu-alert
```

Alarm name:

```text
High-CPU-Alert
```

## Step 5 — Generate CPU Load

Connect to EC2:

```bash
ssh -i key.pem ec2-user@PUBLIC-IP
```

Install `stress-ng`:

```bash
sudo dnf install stress-ng -y
```

Generate CPU load:

```bash
stress-ng --cpu 2 --timeout 300s
```

If `stress-ng` isn't available, a simple demo is:

```bash
yes > /dev/null &
yes > /dev/null &
```

Stop it afterward:

```bash
pkill yes
```

## Expected Flow

```text
CPU Load
   ↓
CPU > 70%
   ↓
CloudWatch Alarm
   ↓
ALARM State
   ↓
SNS Topic
   ↓
Email Notification
   ↓
Administrator takes action
```

## What SNS Does Here

SNS **does not monitor the EC2 instance**.

```text
CloudWatch = Monitoring + Alarm
SNS        = Notification Delivery
Email      = Subscriber
```

This distinction is important when explaining SNS to students.

## More Use-Case-Based SNS Projects

| Project                     | Architecture                          | Level   |
| --------------------------- | ------------------------------------- | ------- |
| **EC2 CPU Alert**           | EC2 → CloudWatch → SNS → Email        | ⭐ Basic |
| **S3 Upload Notification**  | S3 → SNS → Email                      | ⭐ Basic |
| **Application Fan-Out**     | App → SNS → SQS-1 + SQS-2             | ⭐⭐      |
| **Serverless Notification** | App → SNS → Lambda                    | ⭐⭐      |
| **Order Processing**        | Order → SNS → SQS → Billing/Inventory | ⭐⭐⭐     |

For a **first SNS live practical**, I recommend **EC2 → CloudWatch → SNS → Email** because students can clearly see a real operational reason for SNS.

**GitHub repo name:** `aws-sns-cloudwatch-ec2-alert`
