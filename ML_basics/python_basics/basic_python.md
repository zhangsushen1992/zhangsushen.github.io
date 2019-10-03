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

### unique()
```
b = np.unique(a) # display unique values in a
```
If a is a 2-dimensional np array, unique() will outputs a list of unique values in all elements of the 2-d array.
To remove repeating rows in 2-d array:
```
np.array(list(set([tuple(t) for t in c]))
```

### stack() and axis
If we have a, b, c, d of np.array in dimension (2,3):
```
s = np.stack((a,b,c,d), axis=0) # Outputs dimension (4,2,3)
s = np.stack((a,b,c,d), axis=1) # Outputs dimension (2,4,3)
s = np.stack((a,b,c,d), axis=2) # Outputs dimension (2,3,4)
```

### Plotting normal distribution
```
import matplotlib as mpl
mpl.rcParams['font.sans-serif'] = [u'SimHei']
mpl.rcParams['axes.unicode_minus'] = False

mu = 0
sigma = 1
x = np.linspace(mu - 3 * sigma, mu + 3 * sigma, 51)
y = np.exp(-(x - mu) ** 2 / (2 * sigma ** 2)) / (math.sqrt(2 * math.pi) * sigma)

plt.figure(facecolor='w')
plt.plot(x, y, 'r-', x, y, 'gv', linewidth=2, markersize=8, mec='b') # mec is markeredgecolor

plt.xlabel('X', fontsize=15)
plt.ylabel('Y', fontsize=15)
plt.title(u'Normal Distribution', fontsize=18)
plt.grid(True)
plt.show()
```

### Plotting Loss Function
```
plt.figure(figsize=(10,8), facecolor='w')
x = np.linspace(start=-2, stop=3, num=1001, dtype=np.float)
y_logit = np.log(1 + np.exp(-x)) / math.log(2)
y_boost = np.exp(-x)
y_01 = x < 0  # Step function
y_hinge = 1.0 - x
y_hinge[y_hinge < 0] = 0

plt.plot(x, y_logit, 'r-', label='Logistic Loss', linewidth=2)
plt.plot(x, y_01, 'g-', label='0/1 Loss', linewidth=2)
plt.plot(x, y_hinge, 'b-', label='Hinge Loss', linewidth=2)
plt.plot(x, y_boost, 'm--', label='Adaboost Loss', linewidth=2)

plt.grid()
plt.legend(loc='upper right')
plt.savefig('1.png')
plt.show()
```

### Plotting x^x
```
def f(x):
  y = np.ones_like(x)
  i = x > 0
  y[i] = np.power(x[i], x[i])
  i = x < 0
  y[i] = np.power(-x[i], -x[i])
  return y

plt.figure(facecolor='w')
x = np.linspace(-1.3, 1.3, 101)
y = f(x)
plt.plot(x, y, 'g-', label='x^x', linewidth=2)
plt.grid()
plt.legend(loc='upper left')
plt.show()
```

### Plotting Breastline
```
x = np.arange(1, 0, -0.001)
y = (-3 * x * np.log(x) + np.exp(-(40 * (x - 1 / np.e)) ** 4) / 25) / 2
plt.figure(figsize=(5,7), facecolor='w')
plt.plot(y, x, 'r-', linewidth=2)
plt.grid(True)
plt.title(u'Breastline', fontsize=20)
plt.savefig('breast.png')
plt.show()
```

### Plotting Heartline
```
t = np.linspace(0, 2*np.pi, 100)
x = 16 * np.sin(t) ** 3
y = 13 * np.cos(t) - 5 * np.cos(2*t) - 2 * np.cos(3*t) - np.cos(4*t)
plt.plot(x, y, 'r-', linewidth=2)
plt.grid(True)
plt.show()
```

### Plotting Spiral
```
t = np.linspace(0, 50, num=1000)
x = t * np.sin(t) + np.cos(t)
plt.plot(x, y, 'r-', linewidth=2)
plt.grid()
plt.show()
```

### Plotting sin() with Bar
```
x = np.arange(0, 10, 0.1)
y = np.sin(x)
plt.bar(x, y, width=0.04, linewidth=0.2)
plt.plot(x, y, 'r--', linewidth=2)
plt.title(u'Sin curve')
plt.xticks(rotation=-60)
plt.grid()
plt.show()
```
