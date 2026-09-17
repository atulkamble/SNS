```
SNS - Simple Notification Service 

publication, subscription 

topic - name 
subscriptions - 
email 
SMS 

// SNS Practice 

1. create topic 
2. create subscription - add email, mobile >> mail check confirm | mobile, OTP
3. publish message - add text
4. check mail again 
5. delete subscribers, delete topic 

aws sns list-topics
aws sns list-subscriptions
sns list-subscriptions-by-topic --topic-arn arn:aws:sns:us-east-1:535002879962:mytopic
aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:53500287996:mytopic:SUBSCRIPTION-ID

aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:535002879962:GaneshChaturthi:13b80fa7-b7a8-4890-bdea-d71672937ead

arn:aws:sns:us-east-1:535002879962:new


```
