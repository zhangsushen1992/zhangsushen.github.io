## Using Hashlib to Encode Sentences
We import the library:
```
import hashlib
```
Then we encode using md5:
```
md5 = hashlib.md5()
md5.update('This is a sentence.'.encode('utf-8'))
md5.update('This is a second sentence.'.encode('utf-8'))
print('one',md5.digest()) # random code
print('two',md5.hexdigest()) # proper code
```
Printing gives:
- one b'+\xfe\x14I\x16QUL\x05\x99k\x16)$\x85q'
- two 2bfe14491651554c05996b1629248571

We encode the two sentences together:
```
md5 = hashlib.md5()
md5.update('This is a sentence. This is a second sentence.'.encode('utf-8'))
print(md5.hexdigest())
print(md5.digest_size, md5.block_size)
```
Printing gives:
- b627956ed24823e5eea15a7cb9604532
- 16 64

We encode using sha1:
```
sha1 = hashlib.sha1()
sha1.update('This is a sentence.'.encode('utf-8'))
sha1.update('This is a second sentence.'.encode('utf-8'))
print('one',sha1.digest()) # random code
print('two',sha1.hexdigest) # proper code
```
Printing gives:
- one b'\x99\xa7JH\xd5\xfba\x1ae%\x96+\xa3\xd5\x111\xcb:\x1d,'
- two 99a74a48d5fb611a6525962ba3d51131cb3a1d2c

The code is 20 in length, whereas md5 is 16 in length.
We then encode two sentences together.
```
sha1 = hashlib.sha1()
sha1.update('This is a sentence. This is a second sentence.'.encode('utf-8'))
print(sha1.hexdigest())
print(sha1.digest_size, sha1.block_size)
```
Printing gives:
- 41f72cbe7fbda5568448576499ee0507935b7e78
- 20 64

Alternatively we can encode by following:
```
md5 = hashlib.new('md5','This is a sentence. This is a second sentence.'.encode('utf-8'))
print(md5.hexdigest())
sha1 = hashlib.new('sha1','This is a sentence. This is a second sentence.'.encode('utf-8'))
print(sha1.hexdigest())
```
This gives;
- b627956ed24823e5eea15a7cb9604532
- 41f72cbe7fbda5568448576499ee0507935b7e78

Same as the above.
To check what other encoding methods are available, we use:
```
print(hashlib.algorithms_available)
```
