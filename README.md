# AWS SNS (Simple Notification Service) 

## Objective

Learn how to:

* Create an SNS Topic
* Create Email Subscription
* Publish Notifications
* Send Alerts to Multiple Subscribers
* Integrate SNS with AWS Services

---

# What is SNS?

**SNS (Simple Notification Service)** is a fully managed messaging service used to send notifications to subscribers.

### Publisher → Topic → Subscribers

```text
Application
     |
     V
   SNS Topic
   /   |   \
Email SMS Lambda SQS
```

SNS follows a **Pub/Sub (Publisher-Subscriber)** architecture.

---

# Real-Time Use Cases

| Use Case           | Example                        |
| ------------------ | ------------------------------ |
| Monitoring Alerts  | CloudWatch Alarm Notifications |
| Application Alerts | Website Error Notifications    |
| Security Alerts    | Root Login Detection           |
| DevOps Alerts      | Deployment Status              |
| Cost Alerts        | AWS Budget Notifications       |

---

# Architecture

```text
CloudWatch Alarm
        |
        V
    SNS Topic
        |
-------------------
|        |        |
Email1  Email2  Email3
```

---

# Step 1: Create SNS Topic

AWS Console

```text
SNS
→ Topics
→ Create Topic
```

Choose:

```text
Type : Standard
Name : cloudnautic-alerts
```

Click:

```text
Create Topic
```

---

# Step 2: Create Email Subscription

Inside Topic:

```text
Create Subscription
```

Protocol:

```text
Email
```

Endpoint:

```text
your-email@example.com
```

Click:

```text
Create Subscription
```

---

# Step 3: Confirm Subscription

SNS sends email:

```text
AWS Notification - Subscription Confirmation
```

Open email and click:

```text
Confirm Subscription
```

Status changes to:

```text
Confirmed
```

---

# Step 4: Publish Message

Inside Topic:

```text
Publish Message
```

Subject:

```text
Test Alert
```

Message:

```text
Hello Team,

This is a test notification from AWS SNS.

Regards,
AWS Admin
```

Click:

```text
Publish Message
```

---

# Verification

Check email inbox.

Expected Output:

```text
Subject: Test Alert

Hello Team,

This is a test notification from AWS SNS.
```

---

# AWS CLI Practical

## Create Topic

```bash
aws sns create-topic \
    --name cloudnautic-alerts
```

Output:

```json
{
  "TopicArn": "arn:aws:sns:us-east-1:123456789012:cloudnautic-alerts"
}
```

---

## Create Subscription

```bash
aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:123456789012:cloudnautic-alerts \
    --protocol email \
    --notification-endpoint your-email@example.com
```

---

## Publish Message

```bash
aws sns publish \
    --topic-arn arn:aws:sns:us-east-1:123456789012:cloudnautic-alerts \
    --subject "Test Alert" \
    --message "SNS Notification Test"
```

---

# CloudWatch + SNS Practical

## Create SNS Topic

```text
cloudwatch-alerts
```

---

## Create CloudWatch Alarm

Navigate:

```text
CloudWatch
→ Alarms
→ Create Alarm
```

Metric:

```text
EC2 CPU Utilization
```

Condition:

```text
CPU > 80%
```

Action:

```text
Send Notification
```

Choose:

```text
cloudwatch-alerts
```

Create Alarm.

---

# Workflow

```text
EC2 CPU > 80%
      |
      V
CloudWatch Alarm
      |
      V
 SNS Topic
      |
      V
 Email Notification
```

---

# SNS Supported Protocols

| Protocol    | Usage                 |
| ----------- | --------------------- |
| Email       | Notifications         |
| SMS         | Mobile Alerts         |
| SQS         | Queue Integration     |
| Lambda      | Serverless Processing |
| HTTP/HTTPS  | Webhooks              |
| Mobile Push | Mobile Apps           |

---

# SNS vs SQS

| SNS                  | SQS                     |
| -------------------- | ----------------------- |
| Push Model           | Pull Model              |
| Instant Notification | Message Queue           |
| Multiple Subscribers | Single Consumer Pattern |
| Pub/Sub              | Decoupling Service      |

---

# Practical Scenario 1

### Website Down Alert

```text
Website Monitoring Tool
        |
        V
      SNS
        |
        V
    Admin Email
```

Admin receives immediate notification.

---

# Practical Scenario 2

### AWS Budget Alert

```text
AWS Budget > 80%
        |
        V
       SNS
        |
        V
     Email
```

---

# Practical Scenario 3

### Security Alert

```text
Root Login Detected
        |
        V
   CloudWatch Event
        |
        V
       SNS
        |
        V
 Security Team
```

---

# Important Exam & Interview Points

### SNS Characteristics

* Fully Managed Service
* Pub/Sub Messaging
* Push-based Notifications
* Highly Available
* Multi-Subscriber Support
* Integrates with 200+ AWS Services

### Remember

```text
SNS = Push

SQS = Pull
```

```text
One Message
      |
      V
Many Subscribers
```

---

# Cleanup

Delete Subscription:

```bash
aws sns unsubscribe \
    --subscription-arn <subscription-arn>
```

Delete Topic:

```bash
aws sns delete-topic \
    --topic-arn <topic-arn>
```

---

# Hands-On Tasks

### Task 1

Create SNS Topic:

```text
team-alerts
```

Add 2 email subscribers and send a test notification.

### Task 2

Create CloudWatch CPU Alarm and trigger SNS notification.

### Task 3

Create AWS Budget Alert using SNS.

### Task 4

Subscribe an SQS Queue to SNS and verify message delivery.

### Task 5

Create Lambda Trigger from SNS and log messages into CloudWatch Logs.

---

## Key Points to Remember

```text
SNS = Notification Service

Publisher → Topic → Subscriber

Supports:
Email
SMS
SQS
Lambda
HTTP/HTTPS

Push-Based Service

Commonly Used With:
CloudWatch
Budgets
EventBridge
Lambda
Security Alerts
```

This lab covers SNS fundamentals, CLI commands, CloudWatch integration, real-world DevOps alerting scenarios, and interview-focused concepts.

