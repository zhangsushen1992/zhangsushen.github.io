## Deduplication

The notes are from the video https://www.youtube.com/watch?v=McsTWXeURhA which is Flavio Juvenal's presentation in a conference.

### The process of deduplicating:
1. Preprocessing
2. Indexing
3. Comparison
4. Classification
5. Clustering

### General Methods: Preprocessing
1) Put all lower case. 
2) Remove irrelavent separations (commas, dots):
```
irrelevant_regrex = re.compile(r'[^a-z0-9\s]')
company_nmaes = [irrelevant_regrex.sub(' ',c) for c in company_names]
```
3) Remove multi-spaces:
```
multispace_regrex = re.compile(r'\s\s+')
company_names = [multispace_regrex.sub(' ',c).strip() for c in company_names]
```
4) Remove stop words:
```
business_stopwords = {'company', 'co','corporation'}
company_names =[' '.join(c_part for c_part in c.split() if c_part not in business_stopwords]) for c in company_names
```
5) Use probablepeople (not good for non-English)
```
import probablepeople as pp
pp.parse("Apple Computer Incorporated")
company_names_alternative_1 = [[parsed_value for parsed_value, pased_type in pp.parse(c) if parsed_type == 'CoperationName'] for c in company_names_dirty]
```
6) Use nameparser
```
from nameparser improt HumanName
form IPython.display import display
display(pp.parse("Fiavio Juvenal da Silva Jr"))
display(HumanName("Fiavio Juvenal da Silva Jr"))
```
7) Remove accent:
```
import unidecode
unidecode.unidecode('Flavio')
```
8) Clean Address
```
import requests
import geocoder
full_addresses_latlng=[]
with requests.Session() as session:
    for a in full_addresses:
        a_geocoded = geocoder.google(a, session=session)
        full_addresses_lating.append(a.geocoded.latlng)
address_latlng = list(zip(full_addresses, full_addresses_latlng))
```
But geocoder is a web API with quota limit. Alternatively build your own geocoder at :
github.com/openstreetmap/Nominatim
github.com/pelias/pelias
github.com/kommot/photon
postgis.net/docs/Extras.html
github.com/openvernues/lieu
Alternatively we can use pypostal:
```
from postal.expand import expand_address
expnad_address(full_addresses[8]) 
```
The package does not expand address to a single expansion, when there is uncertainty it will expand to different possiblities
9) Clean phone numbers:
```
import phonenumbers
phone='(541)555-3010'
phonenumbers.format_number(phonenumbers.parse(phone,'US'), phonenumbers.PhoneNumberFormat.E164))
```
10) Clean dates
```
import dateparser
dateparser.parse('at 10/1/2019 10am')
dateparser.DateDataParser().get_date_data('at 10/1/2019 10am')
```
### Example:
```
def assign_no_symbols_name(df):
    return df.assign(
        name=df['name'].str.replace(irrelevant_regrex, ' ').str.replace(multispace_regrex, ' '))
df = assign_no_symbols_name(df)
df.head(9)
```
```
from collections import Counter
possible_stopwords = Counter(" ".join(df['name']).split()).most_common(20)
pprint.pprint(possible_stopwords)
```
```
def assign_cleaned_name(df):
    restaurant_stopwords={'a','the','la','le','of','and', 'on','l'}
    restaurant_stopwords_regrex = r'\b(?:{})\b'.format('|'.join(restaurant_stopwords))
    return df.assign(name=df['name']
                        .str.replace(restaurant_stopwords_regrex, '')
                        .str.replace(multiplespace_regrex,'')
                        .str.strip())
df=assign_cleaned_name(df)
df.head(9)
```
```
def assign_postal_lat_lng(df):
    addresses = df['addr'].str.cat(df['city'],sep=', ')
    addresses_to_postal = [address_to_postal.get(a) for a in addresses]
    addresses_to_lat = [address_to_latlng[a][0] if a in address_to_latlng else None for a in addresses]
    addresses_to_lng = [address_to_latlng[a][1] if a in address_to_latlng else None for a in addresses]
    
    return df.assign(postal=addresses_to_postal, lat=addresses_to_lat, lng=addresses_to_lng)
df = assign_postal_lat_lng(df)
df.head(6)
```
```
def assign_addr_variations(df):
    return df.assign(
        addr_variations = df['addr'].apply(lambda addr: frozenset(exapnd_address(addr))))
df = assign_addr_variations(df)
df[['name','addr','addr_variations']].head(6)
```
### General Methods: Indexing
The Record Linkage Toolkit produces pairs of all records against all records to make a match.
```
import recordlinkage as rl
full_indexer = rl.FullIndex()
pairs = full_indexer.index(df)
```
To produce pairs with equal values for postal code:
```
postal_indexer = rl.BlockIndex('postal')
postal_index_pairs = postal_indexer.index(df)
```
To produce pairs that have neighbouring values for name:
```
name_indexer = rl.SortedNeighbourIndex('name', window=7)
name_index_pairs = name_indexer.index(df)
```

### General Methods: Comparison
To define similarity functions:
```
vectorized_intersection = np.vectorize(lambda x,y: float(bool(x.intersection(y))))

def compare_addr_variations(a1,a2):
    return vectorized_intersection(a1,a2)
    
comp = rl.Compare()
comp.string('name','name', method='jarowinkler', label='name')
comp.string('addr','addr', method='jarowinkler', label='addr')
comp.string('city','city', method='jarowinkler', label='addr')
comp.geo('lat','lng','lat',lng', method='exp', scale=0.5, label='latlng')
comp.compare_vectorized(compare_addr_variations, 'addr_variations', 'addr_variations', label='addr_variations')
```
A few similarity functions:
jarowinkler gives priority to the beginning of the string
levenshtein cares more about order
lcs cares less about order
To compare different algorithms:
```
for recordlinkage.algorithms.string import(
    jarowinkler_similarity as jarowinkler,
    levenshtein_similarity as levenshtein,
    longest_common_substring_similarity as lcs)
    
for s1, s2 in [['the palm','plam the']]:
    print(jarowinkler([s1],[s2])[0]
```
```
comparison_vectors = comp.compute(pairs,df)
comparison_vectors.head(5)
```

### Genearl Methods: Classification
Threshold based classification: weighted average over vectors
```
scores = np.average(comparison_vectors.values, axis=1, weights=[30,10,5,10,30,15])
scored_comparison_vectors = comparison_vectors.assign(score=scores)
scored_comparison_vectors.head(5)
```
```
matches = comparison_vectors[scored_comparison_vectors['score'] >= 0.85]
matches.head(5)
```

