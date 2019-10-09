## AWS and Boto3 Notes 2
### Alerting and Notification - SNS service
To create SNS Topic:
```
sns = boto3.client('sns', region_name='us-east-1', aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
response = sns.create_topic(Name='city_alerts')
topic_arn = response['TopicArn']
# Alternatively a short cut: sns.create_topic(Name='city_alerts')['TopicArn']
```
To list all topics:
```
response = sns.list_topics()
```
To delete topics, use topic ARN:
```
sns.delete_topic(TopicArn='arn:aws:sns:us-east-1:320333787981:city_alerts')
```
To manage subscription, SMS or email:
```
response = sns.subscribe(TopicARn='arn:aws:sns:us-east-1:320333787981:city_alerts', Protocol='SMS', Endpoint='+13125551123')

response = sns.subscribe(TopicARn='arn:aws:sns:us-east-1:320333787981:city_alerts', Protocol='email', Endpoint='max@maksimize.com')
```
To list subscriptions by Topic or list all:
```
sns.list_subscriptions_by_topic(TopicArn='arn:aws:sns:us-east-1:320333787981:city_alerts')
sns.list_subscriptions()['Subscriptions']
```
To delete subscriptions:
```
sns.unsubscribe(SubscriptionArn='arn:aws:sns:us-east-1:320333787981:city_alerts:9f6d-j28sh9dtb2')
```
To delete multiple subscriptions (delete all subscriptions with sms service:
```
response = sns.list_subscriptions_by_topic(TopicArn='arn:aws:sns:us-east-1:320333787981:city_alerts')
subs = response['Subscriptions']

for sub in subs:
  if sub['Protocol'] == 'sms':
    sns.unsubscribe(sub['SubscriptionArn'])
```
