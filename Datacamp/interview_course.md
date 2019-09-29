## Interview Course Notes 1
### str() constructor
```
class NewClass:
  def __init__(self,num):
    self.num = num

nc = NewClass(2)
print(nc.num)
```

If we have str(nc), there is an error generated.
Therefore, we need to add the __str__() method.
```
class NewClass:
  def __init__(self,num):
    self.num = num
  
  def __str__(self):
    return str(self.num)

nc = NewClass(3)
str(nc)
```
This gives string '3'.

The .index() method returns the index of a character.
```
s = 'interview'
s.index('n') # generates 1
s.index('i') # generates 0
```
Strings are immutable
```
s[0] = 'a' # Error
```

Replace a substring:
```
s1 = 'a dog ate my food'
s2 = s1.replace('dog','cat')
```
Printing gives 'a cat ate my food'.

Upper case, lower case and capitalise:
```
s3 = s2.upper()
s4 = s2.lower()
s5 = s2.capitalize() #First letter upper case
```

Joining and spliting:
```
l = ['I','like','studying']
s = ' '.join(l)
print(s)
```
This gives 'I like studying'.
```
l = s.split(' ')
print(l)
```
This gives ['I','like','studying'].

In a DataFrame, we can also use these methods.
```
D['name'] = D['name'].str.capitalize()
```
To count the number of items in different values:
```
D['name'].value_counts()
```

### Regular Expressions
Mapping Rules:
a -> a
A -> A
1 -> 1
. -> any character
\. -> .
\w -> any alphanumeric character or underscore '1', 'a', '_', ...
\d -> any digit '1', '2', '3', ...
\s -> any whitespace character ' ', '\t', ...
[aAbB] -> a, A, b, B
[a-z] -> a, b, c, ...
[A-Z] -> A, B, C, ...
[0-9] -> 0, 1, 2, ...
[A-Za-z] -> A, B, C, ..., a, b, c, ...
a* -> no or repeated undefined times ' ', 'a', 'aa', ...
a+ -> at least once 'a', 'aa', 'aaa', ...
a? -> exists or not ' ', 'a'
{n, m} character present from n to m times a{2,4} -> 'aa', 'aaa', 'aaaa'

Emails, for example, can be written as:
```
[\w\.]+@[a-z]+\.[a-z]+
```

Use re package:
```
import re
pattern = re.compile(r'[\w\.]+@[a-z]+\.[a-z]+')
text = 'john.smith@mailbox.com is the email of '\
'John. He often writes to his boss at '\
'boss@company.com. But the message get forwarded '\
'to his secretary at info@company.com.'
result = re.finditer(pattern, text)
print(result) # <callable_iterator object at 0x7f5dff81af98>
for match in result:
  print match
```
This gives the three emails contained in the text as Match objects.
To extract values from Match object:
```
for match in result:
  print(match.group()) #generates email address
  print(match.start()) #generates the starting position of matched email address
  print(match.end()) #generates the ending position of matched email address
```
To find a list of emails from the text:
```
substrings = re.findall(pattern, text)
print(substrings)
```
This returns a list of all email addresses contained in text.
```
split_list = re.split(pattern, text)
print(split_list)
```
This generates text separated by email addresses as lists of strings:
[' ', 'is the email of John. He often writes to his boss at ', '. But the messages get forwarded to his secretary at ',
'.']
