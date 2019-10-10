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

### Rekognition package
To initialise C3 client:
```
s3 = boto3.client('s3', region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
```
To upload a picture:
```
s3.upload_file(Filename='report.jpg', Key='report.jpg', Bucket='datacamp-img')
```
To initialise the client and perform detection for image labelling, MaxLabels is the maximum number of labels generated, MinConfidence is how confident the label is:
```
rekog = boto3.client('rekognition', region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
response = rekog.detect_labels(Image={'S3Object': {'Bucket': 'datacamp-img', 'Name': 'report.jpg'}}, MaxLabels=10, MinConfidence=95)
```
For text detection:
```
response = rekog.detect_text(Image={'S3Object':{'Bucket':'datacamp-img', 'Name':'report.jpg'}}
```
Example:
```
# Iterate over the labels in the response
for label in response['Labels']:
    # Find the cat label, look over the detected instances
    if label['Name'] == 'Cat':
        for instance in label['Instances']:
            # Only count instances with confidence > 85
            if (instance['Confidence'] > 85):
                cats_count += 1
# Print count of cats
print(cats_count)
```
Example:
```
# Create empty list of words
words = []
# Iterate over the TextDetections in the response dictionary
for text_detection in response['TextDetections']:
  	# If TextDetection type is WORD, append it to words list
    if text_detection['Type'] == 'WORD':
        # Append the detected text
        words.append(text_detection['DetectedText'])
# Print out the words list
print(words)
```

Comprehend test: To initialise client:
```
translate = boto3.client('translate', region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
```
To translate text:
```
response = translate.translate_text(Text='Hello, how are you?', SourceLanguageCode='auto', TargetLanguageCode='es')['TranslatedText']
```
To comprehend text: To initialise client:
```
comprehend = boto3.client('comprehend',region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
```
To detect language:
```
response = comprehend.detect_dominant_language(Text='Hay basura por todas partes a lo largo de la carretera.')
```
Use comprehend to detect sentiment:
```
response = comprehend.detect_sentiment(Text='DataCamp students are amazing.', LanguageCode='en')['Sentiment']
```
Example:
```
# For each dataframe row
for index, row in df.iterrows():
    # Get the public description field
    description = df.loc[index, 'public_description']
    if description != '':
        # Detect language in the field content
        resp = comprehend.detect_dominant_language(Text=description)
        # Assign the top choice language to the lang column.
        df.loc[index, 'lang'] = resp['Languages'][0]['LanguageCode']
        
# Count the total number of spanish posts
spanish_post_ct = len(df[df.lang == 'es'])
# Print the result
print("{} posts in Spanish".format(spanish_post_ct))
```
Example:
```
for index, row in dumping_df.iterrows():
  	# Get the public_description into a variable
    description = dumping_df.loc[index, 'public_description']
    if description != '':
      	# Translate the public description
        resp = translate.translate_text(
            Text=description, 
            SourceLanguageCode='auto', TargetLanguageCode='en')
        # Store original language in original_lang column
        dumping_df.loc[index, 'original_lang'] = resp['SourceLanguageCode']
        # Store the translation in the translated_desc column
        dumping_df.loc[index, 'translated_desc'] = resp['TranslatedText']
# Preview the resulting DataFrame
dumping_df = dumping_df[['service_request_id', 'original_lang', 'translated_desc']]
dumping_df.head()
```

### Case Study
Initialise rekognition client:
```
rekog = boto3.client('rekognition',region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
comprehend = boto3.client('comprehend',region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
translate = boto3.client('translate',region_name='us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_acces_key=AWS_SECRET)
```
Translate all descriptions to English:
```
for index, row in df.iterrows():
  desc = df.loc[index, 'public_description']
  if desc != '':
    resp = translate_fake.translate_text(Text=desc, SourceLanguageCode='auto', TargetLanguageCode='en')
    df.loc[index,'public_description'] = resp['TranslatedText']
```
Detect sentiment in the text:
```
for index, row in df.iterrows():
  desc = df.loc[index, 'public_description']
  if desc != '':
    resp = comprehend.detect_sentiment(Text=desc, LanguageCode='en')
    df.loc[index,'sentiment'] = resp['Sentiment']
```
Detect scooter in image:
```
df['img_scooter']=0
for index, row in df.iterrows():
  image = df.loc[index, 'image']
  response = rekog.detect_labels(Image={'S3Object':{'Bucket':'gid-images','Name':image}})
  for label in response['Labels']:
    if label['Name'] == 'Scooter':
      df.loc[index, 'img_scooter'] = 1
      break
  ```
  Select rows where there was a scooter image and negative sentiment:
  ```
  pickups = df[((df.img_scooter == 1) & (df.sentiment == 'NEGATIVE'))]
  num_pickups = len(pickups)
  ```
