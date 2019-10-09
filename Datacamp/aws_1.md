## AWS and Boto3 Notes 1
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
