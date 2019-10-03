## Notes on Basic Python 2
### Uniform Distribution
```
x = np.random.rand(10000)
t = np.arange(len(x))
plt.hist(x, 30, color='m', alpha=0.5, label=u'Uniform Distribution')
# The following line plots a scatter of randomly generated points from a uniform distribution
# plt.plot(t, x, 'g.', label=u'Uniform Scatter')
plt.legend(loc='upper left')
plt.grid()
plt.show()
```

### Proof of Central Limit Theorem
```
t = 1000
a = np.zeros(10000)
for i in range (t):
  a += np.random.uniform(-5, 5, 10000)
a /= t
plt.hist(a, bins=30, color='g', alpha=0.5, normed=True, label='Uniform Stacking')
plt.legend(loc='upper left')
plt.grid()
plt.show()
```
