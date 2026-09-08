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

# AWS SNS — Use Cases & Architecture Flow

![Image](https://images.openai.com/static-rsc-4/vCdmvVSh8B3Z5pUO-9Q8MPeMdm9byYtcrjtoMQZJIP7G_wjjWnSeTBgN7XZeB8Wg5C2NyUrskEowG_b0YrN9RRY3gq9mVEkHBj8UJ2Wbev8171dtTyqyWii6fASTo40hkOyp4MjAkXzibAq3p2qvKeum8AzxD5xkreWXRHHK18psLdDpuOT6jXsvHD4vAizz?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Xtx3oWRb0e-NXoyCVFJknWkB6IHVX_7N_G_ClfAXhgGonZldE9TvV3yejdX-T9lLkcDirP4ixIlgpuZZBtLOCoZx16VN4zjGwTocYaF3c4mL0ss9DtHyhexsY3v7-oD6BjLxTlaqsn_8Mlikghjw_-iJ69iYUGh1MglHGjjdopOzXUJtD_z1BAAUIwhmA9TE?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Js8CLJ-Vhg1q1bp9OUgN61MfZfIBm5AJ93a0WUZ4VIPbUT1l3jusxA2jFifuWvBA5lP5TsPF0k0IBosUCEdHmXBNFEz8f0yTswYT2_wOyOxsrzRB1StLQxkPjESMOsVQ0p3QBiJW1JxQktzeZdjIgAaaNEh0sniRltqMxPi2SJOhEkW6Loy67u_I6sRpJQRM?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/577Ld5IyJxSgT0uKERXHIxWSgHBjWHvdGqF61ftHCh-U6YyqXdlADkhiRiGAhSR5dM_CxUVRiaRjXO7KBodN9YWp3HK6Jhb9uQAeoeAMev98rjrOvbRUOZBLiRfj4Tlerm5293jfkYqE1A9HsRLm1_P-PVpyhaC-x20UWGvBopZoOzWFO3lfHNmAP2ZIFJ6a?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/PGryxao-2O-UZeo26huKskc0B9XAp-2TTfW9LYfbpxZNsGMlkLzRPXIxURexcoyJB30GcSpAVl97xr1kUE1yulLUephVBlYOZ4cUQz6QhUiPoNcBUZ6PanN_hMRHHgFB1KPrLCnaYZ-c5sp4OxwG8iKtgbXxwlOqFaxMCI_Po9iXZ3j2JVkQ0WFqzexXMyxE?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/6qRHkrDRYoWTN-xIpak0KXq7eW4HBJDKwzDqkjHRzmqFK3CQyr6TGw32XvJLM7Co2xDdIihzym1XrHwhDZk9bG-9XwaMg1Zq_uKQTb3GbDfJamuomHKmdtmVhTxTkCd4ihWo17lgWl-jR2GPV1xVwhtgkG90tH75Abjsx9IidcpGXUV_0pNN9JbV12kM84AP?purpose=fullsize)

## What is Amazon SNS?

**Amazon Simple Notification Service (SNS)** is a **fully managed Pub/Sub messaging service**.

**Basic idea:**
**Publisher → SNS Topic → Multiple Subscribers**

One message published to a topic can be delivered to **many subscribers simultaneously**.

### Architecture Flow

```text
                 Publisher
             Application / AWS
                     │
                     │ Publish Message
                     ▼
              ┌──────────────┐
              │  SNS Topic   │
              └──────┬───────┘
                     │
              Fan-Out Message
         ┌───────────┼───────────┬───────────┐
         ▼           ▼           ▼           ▼
       Email        SMS         SQS        Lambda
         │           │           │           │
         ▼           ▼           ▼           ▼
       User        Mobile      Worker      Function
```

### Common SNS Use Cases

| Use Case                     | Example Flow                                      |
| ---------------------------- | ------------------------------------------------- |
| 📧 Email notification        | CloudWatch → SNS → Email                          |
| 📱 SMS alerts                | Application → SNS → SMS                           |
| 🚨 Infrastructure alerts     | CloudWatch Alarm → SNS → Admin                    |
| ⚡ Serverless processing      | Application → SNS → Lambda                        |
| 📦 Queue fan-out             | SNS → Multiple SQS Queues                         |
| 🛒 E-commerce events         | Order Service → SNS → Inventory + Payment + Email |
| 🔔 Application notifications | App → SNS → Multiple subscribers                  |
| 🔗 Webhook/API integration   | SNS → HTTP/HTTPS endpoint                         |

## Important Architecture: SNS Fan-Out

```text
                         ┌──→ SQS → Order Processing
                         │
Order Service → SNS Topic├──→ SQS → Inventory
                         │
                         ├──→ Lambda → Send Notification
                         │
                         └──→ Email → Administrator
```

This is called **Fan-Out Architecture**: publish the event **once**, then SNS distributes it to multiple independent consumers.

### Real-World Example: Online Order

```text
Customer
   │
   ▼
Place Order
   │
   ▼
Order Application
   │
   ▼
SNS Topic: "NewOrder"
   │
   ├────→ SQS ───→ Payment Service
   │
   ├────→ SQS ───→ Inventory Service
   │
   ├────→ Lambda → Analytics
   │
   └────→ Email ─→ Operations Team
```

### SNS vs SQS — Remember This

**SNS = Push + Pub/Sub + Fan-out**
**SQS = Queue + Pull + Message buffering**

A very common AWS architecture combines them:

**Producer → SNS → SQS → Consumers**

This gives you **SNS fan-out** plus **SQS durability, buffering, and independent processing**.
