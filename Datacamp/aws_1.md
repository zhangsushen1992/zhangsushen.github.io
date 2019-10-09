## AWS and Boto3 Notes 1
### AWS basics
To set up the client:
```
import boto3
s3 = boto3.client('s3', region_name = 'us-east-1',aws_access_key_id=AWS_KEY_ID, aws_secret_access_key=AWS_SECRET)
response = s3.list_buckets()
```
To create bucket:
```
bucket = s3.create_bucket(Bucket='gid-requeests')
```
To list buckets:
```
bucket_response = s3.list_buckets()
buckets = bucket_response['Buckets']
print(buckets)
```
To delete buckets:
```
response = s3.delete_bucket('gid-response')
```
To upload file, key is the name in buckets, filename is the name in local disk:
```
s3.upload_file(Filename='gid_requests_2019_01_01.csv',Bucket='gid-requests',Key='gid_requests_2019_01_01.csv')
```
To list objects in a bucket (list objects starting with gid_requests_2019_, maxkey determines the number of files to output):
```
response = s3.list_objects(Bucket='gid-requests',MaxKeys=2, Prefix='gid_requests_2019_')
print(response)
```
To download files:
```
s3.download_file(Filename='gid_requests_downed.csv',Bucket='gid-requests',Key='gid_requests_2018_12_30.csv')
```
To delete objects:
```
s3.delete_object(Bucket='gid-requests',Key='gid_requests_2018_12_30.csv')
```
To obtain object metadata:
```
response = s3.head_object(Bucket='gid-requests;,Key='gid_requests_2018_12_30.csv')
print response
```

### Set permissions to objects
To set ACL to 'public-read':
```
s3.put_object_acl(Bucket='gid-requests',Key='potholes.csv', ACL='public-read')
```
To set ACL in upload:
```
s3.upload_file(Bucket='gid-requests', Filename='potholes.csv', Key='potholes.csv', ExtraArgs={'ACL':'public-read'})
```
To access public objects:
```
https://{bucket}.s3.amazonaws.com/{key}
```
Or:
```
url = "https://{}.s3.amazonaws.com/{}".format("gid-requests","2019/potholes.csv")
df = pd.read_csv(url)
```
AWS Permission system:
- IAM: what can this user do in AWS
- Bucket Policy: who can access this bucket
- ACL: who can access this object
- Presigned URL: temporary access

To read private files:
```
obj = s3.get_object(Bucket='gid-requests', Key='2019/potholes.csv')
print(obj)
# files are read with pandas
pd.read_csv(obj['Body'])
```
Alternatively, private files can be accessed through presigned urls. To generate presigned url (expires in 1 hour):
```
share_url = s3.generate_presigned_url(ClientMethod='get_object', ExpiresIn=3600, Params={'Bucket':'gid-requests','Key':'potholes.csv'})
pd.read_csv(share_url)
```
To load multiple files into one DataFrame:
```
df_list = []
response = s3.list_objects(Bucket='gid-requests',Prefix='2019/')
request_files = response['Contents']

for file in request_files:
  obj = s3.get_object(Bucket='gid-requests',Key=file['Key'])
  obj_df = pd.read_csv(obj['Body']
  df_list.append(obj_df)
  
df = pd.concat(df_list)
df.head()
```

Pandas can write data to html:
```
df.to_html('table_agg.html')
# To make any url clickable:
df.to_html('table_agg.html',render_links=True)
```
To convert certain columns:
```
# border=0 remove border, border=1 show border
df.to_html('table_agg.html',render_links=True, columns['service_name','request_count','info_link'],border=0)
```
To upload html file to S3:
```
s3.upload_file(Filename='./table_agg.html', Bucket='datacamp-website', Key='table.html', ExtraArgs={'ContentType':'text/html', 'ACL':'public-read'})
```
To upload image file to S3:
```
s3.upload_file(Filename='./plot_image.png', Bucket='datacamp-website', Key='plot_image.png', ExtraArgs={'ContentType':'image/png', 'ACL':'public-read'})
```
IANA Media Types decide file type based on extension. Common types include json, png, pdf, csv.
To generate an index page:
```
r = s3.list_objects(Bucket='gid-reports', Prefix='2019/')
objects_df = pd.DataFrame(r['Contents'])

# create a column 'link' that contains website url+key
base_url = "http://datacamp-website.s3.amazonaws.com/"
objects_df['Link'] = base_url + objects_df['Key']

# write dataframe to html
objects_df.to_html('report_listing.html', columns=['Link', 'LastModified', 'Size'], render_links=True)

# upload html to S3
s3.upload_file(Filename='./report_listing.html', Bucket='datacamp-website', Key='index.html', ExtraArgs={'ContentType: 'text/html', 'ACL':'public-read'})
```

### Case Study
Read raw data files:
```
df_list = []
response = s3.list_objects(Bucket='gid-requests', Prefix='2019_jan')
requests_files = response['Contents']

for file in requests_files:
  obj = s3.get_object(Bucket='gid-requests', Key=file['Key'])
  obj_df = pd.read_csv(obj['Body'])
  df_list.append(obj_df)
  
df = pd.concat(df_list)
df.head()

df.to_csv('jan_final_report.csv')
df.to_html('jan_final_report.html')
```
To upload aggreagated csv, html, and chart:
```
s3.upload_file(Filename='./jan_final_report.csv', Key='2019/jan/final_report.csv', Bucket='dig-reports', ExtraArgs={'ACL':'public-read'})
s3.upload_file(Filename='./jan_final_report.html', Key='2019/jan/final_report.html', Bucket='dig-reports', ExtraArgs={'ContentType':'text/html', 'ACL':'public-read'})
s3.upload_file(Filename='./jan_final_chart.html', Key='2019/jan/final_chart.html', Bucket='dig-reports', ExtraArgs={'ContentType':'text/html','ACL':'public-read'})
```
To create index.html:
```
r = s3.list_objects(Bucket='gid-reports', Prefix='2019/')
objects_df = pd.DataFrame(r['Contents'])
base_url = "https://gid-reports.s3.amazonaws.com/"
objects_df['Link'] = base_url + objects_df['Key']

objects_df.to_html('report_listing.html', columns=['Link','LastModified','Size'], render_links=True)

s3.upload_file(Filename='./report_listing.html', Key='index.html', Bucket='gid-reports', ExtraArgs={'ContentType':'text/html', 'ACL':'public-read'})
```
