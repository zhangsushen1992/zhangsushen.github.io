## Intermediate Python for Data Science Notes 1
### Matplotlib
```
import matplotlib.pyplot as plt
plt.scatter(gdp_cap, life_exp)
plt.xscale('log')
plt.yticks([0,2,4,6,8,10],['0','2B','4B','6B','8B','10B']) # change the tick displays at 0,2,4... to strings with unit 'B'
plt.show()
```
To draw histogram::
```
plt.hist(values, bin=3)
plt.show()
plt.clf() #cleans the current figure to draw a new figure
```
To draw scatter of gdp vs life expectancy with point size as population size (s=np.array(pop)) and setting colours (c=col) and setting transparency (alpha=0.8), displaying grid and label of China and India:
```
plt.scatter(x = gdp_cap, y = life_exp, s = np.array(pop) * 2, c=col, alpha=0.8)
plt.text(1550, 71, 'India')
plt.text(5700, 80, 'China')
plt.grid(True)
```

### Dictionaries
When we do not have dicts, we index list with the following code:
```
countries = ['spain', 'france', 'germany', 'norway']
capitals = ['madrid', 'paris', 'berlin', 'oslo']

ind_ger=countries.index('germany')
capitals[ind_ger]
```
When we have dicts:
```
europe = {'spain':'madrid', 'france':'paris', 'germany':'berlin', 'norway':'oslo' }

# Print out the keys in europe
print(europe.keys())
# Print whether italy is the key in europe
print('italy' in europe)
```
Note: Keys to a dict must be immutable (list is not okay).
