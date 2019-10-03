## Notes on Basic Python
### reshape()
reshape() can change the shape of new array without changing the shape of old array. 
```
b = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
c = b.reshape(4,-1)
```
However, b and c share the same memroy, such that if you change the value of b, the corresponding value in c is changed.
```
b[0][1]=20
print(c)
```
c is now [[1, 20, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]].

### dtype and astype()
When defining np array, we can specify the type. Using astype() can change data types.
```
b = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]], dtype=np.float)
d = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]], dtype=np.complex)
f = d.astype(np.int)
```

### print(__file__)
```
print(__name__) # output '__main__'
print(__file__) # output file location
```
If ```print(__name__)``` is called from another file, it will output the name of the file.

### set_printoptions()
```
np.set_printoptions(linewidth=100) # default linewidth=80
np.set_printoptions(suppress=True) # the float digits are suppressed in display
```

### Create array
Using arange(), we define the step length. Using linspace() or logspace(), we define the number of elements. The endpoint attribute defines whether the end value of 10 is included.
```
a = np.arange(1, 10, 0.1)
b = np.linspace(1, 10, 50, endpoint=False)
c = np.logspace(1, 4, 4, endpoint=True, base=2)
d = np.Logspace(0, 10, 11, endpoint=True, base=2)
```

### fromstring()
The following generates the list of ASCI code for 'abcd'.
```
s = 'abcd'
g = np.fromstring(s, dtype=np.int8)
```

### Slicing
```
a[::-1] # display reverse order
a[0:6:2] # step length 2 from 0 to 5
a[9:1:-2] # step length 2 reverse order
```
Slicing shares the memory.
```
b = a[2:5]
b[0] = 200 # a[2] is changed to 200
```
However, in the following case, a copy is done.
```
a = np.logspace(0, 9, 10, base=2)
i = np.arange(0, 10, 2)
b = a[i]
b[2] = 1.6
print(a) # the value in a is not changed
```

### Indexing
```
b = a[a > 0.5]
a[a > 0.5] = 0.5
```

### Broadcasting
```
a = np.arange(0, 60, 10).reshape(-1,1) # Column
b = np.arange(6) # Row
c = a + b
```
c is a 6x6 matrix.

### Slicing of 2-d matrix
If a is a matrix:
[[0 1 2 3 4 5]
[10 11 12 13 14 15]
[20 21 22 23 24 25]
[30 31 32 33 34 35]
[40 41 42 43 44 45]
[50 51 52 53 54 55]]
```
print(a[[0,1,2],[2,3,4]]) # Outputs [2 13 24]
print(a[4,[2,3,4])        # Outputs [42 43 44]
print(a[4:,[2,3,4]])      # Outputs [[42 43 44][52 53 54]]
i = np.array([True, False, True, False, False, True)
print(a[i])               # Outputs [[0 1 2 3 4 5][20 21 22 23 24 25][50 51 52 53 54 55]]
print(a[i,3])             # Outputs [3 23 53]
```

