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

To publish a Topic, subject line is only visible to email subscribers:
```
response = sns.publish(TopicArn='arn:aws:sns:us-east-1:320333787981:city_alerts', Message='Body text of SMS or email', Subject='Subject line for email')
```
To custom meassge:
```
num_of_reports = 137
response = sns.publish(TopicArn='arn:aws:sns:us-east-1:320333787981:city_alerts', Message='There are {} reports outstanding'.format(num_of_reports), Subject='Subject line for email')
```
To send a single SMS:
```
response = sns.publish(PhoneNumber='+13121233211', Message='Body of text of SMS or email')
```

### Case Study
Step 1: set up topics
```
sns = boto3.client('sns', region_name='us-east-1', aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
trash_arn = sns.create_topic(Name='trash_notifications')['TopicArn']
streets_arn = sns.create_topic(Name='streets_notifications')['TopicArn']
```
Step 2: create alerts
```
contacts = pd.read_csv('http://gid-staging.s3.amazonaws.com/contacts.csv')

def subscribe_user(user_row):
  if user_row['Department'] == 'trash':
    sns.subscribe(TopicArn=trash_arn, Protocol='sms', Endpoint=str(user_row['Phone'])
    sns.subscribe(TopicArn=trash_arn, Protocol='email', Endpoint=str(user_row['Email'])
  else:
    sns.subscribe(TopicArn=street_arn, Protocol='sms', Endpoint=str(user_row['Phone'])
    sns.subscribe(TopicArn=street_arn, Protocol='email', Endpoint=str(user_row['Email'])
  
  contacts.apply(subscribe_user, axis=1)
  ```
  To get aggregatec numbers:
  ```
  df = read_csv('http://gid-reports.s3.amazonaws.com/2019/feb/final_report.csv')
  df.set_index('service_name', inplace=True)
  trash_violations_count = df.at['Illegal Dumping', 'count']
  streets_violations_count = df.at['Pothole','count']
  
  if trash_violations_count > 100:
    message = "Trash violations count is now {}'.format(trash_violations_count)
    sns.publish(TopicArn = trash_arn, Message=message, Subject='Trash Alert')
    

  if streets_violations_count > 100:
    message = "Streets violations count is now {}'.format(streets_violations_count)
    sns.publish(TopicArn = street_arn, Message=message, Subject='Streets Alert')
```
