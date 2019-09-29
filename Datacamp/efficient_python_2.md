## Efficient Python Notes 2
### Efficiently comibining two lists
If we have two lists as follows:
```
names = ['Bulbasaur', 'Charmander' , 'Squirtle']
hps = [45, 39, 44]
```
To combine the list into [('Baulbasaur',45),('Charmander',39),('Squirtle',44)], we use the following function:
```
combined_zip = zip (names, hps)
combined_zip_list = [*combined_zip]
```
### Counting the number of repeated items in a list
We use the Counter object from the collections module.
```
poke_types = ['Grass', 'Dark', 'Fire', 'Fire'...]
from collections import Counter
type_counts = Counter(poke_types)
print (type_counts)
```
This outputs the following which is ranked in descending order:
```
Counter({'Water':105, 'Normal':92, 'Bug':65...})
```
### Generating combinations of pairs from a list
We use itertools.combinaitons() to achieve this.
```
poke_types =['Bug','Fire','Ghost', 'Grass','Water']
from itertools import combinations
combos_obj = combinations(poke_types,2)
combos =[*combos_obj]
print(combos)
```
Note that combinations outputs a combinations class instance. To unpack into list, we need the * operation.
The output is:
```
[('Bug','Fire'),('Bug','Ghost'),('Bug','Grass')...]
