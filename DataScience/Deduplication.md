## Deduplication

The notes are from the video https://www.youtube.com/watch?v=McsTWXeURhA which is Flavio Juvenal's presentation in a conference.

### The process of deduplicating:
1. Preprocessing
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
2. Indexing
3. Comparison
4. Classification
5. Clustering
