# Python Webscrapping with CBC News

### It could be quite time consuming to read news one by one. 
### In this exercise I would be using beautiful soup to get te news title and divide the region.

First Install Beautiful Soup
```
pip install requests beautifulsoup4 requests-html
```
Then import the libraries that we might need to use potentially
```
import pandas as pd
from urllib.parse import urlparse
import numpy as np
import nltk.data
from requests_html import HTMLSession
import requests
from bs4 import BeautifulSoup
```
Once getting response 200 we are good to start
```
url = requests.get('https://www.cbc.ca/news/canada')
soup = BeautifulSoup(url.content, 'html.parser')
```
As all the news head  title is stored uner 'h3.headline' it would be our selector
```
news_titles = soup.select('h3.headline')
```
Using for loop to get all the titles on top page
```for news_title in news_titles:
    print(news_title.text)
```
Then use pandas library to make a table
```
news_titles_df = pd.DataFrame(news_titles)
```

###2 Divide region on Canada Column

From scrapping the link we are able to read the pattern of the html

the py script
```
for i in soup.find_all('a', href=True):
    if 'news/canada' in i['href']:
        print('https://www.cbc.ca/'+i['href'])
```
The result
```
https://www.cbc.ca//news/canada/beingblackincanada
https://www.cbc.ca//news/canada/edmonton/first-nation-hunting-access-jasper-park-alberta-1.7014906
https://www.cbc.ca//news/canada/edmonton/first-nation-hunting-access-jasper-park-alberta-1.7014906
https://www.cbc.ca//news/canada/calgary/alberta-energy-storage-danielle-smith-fantasy-vs-reality-1.7012795
https://www.cbc.ca//news/canada/toronto/mandatory-holocaust-education-schools-1.7015118
https://www.cbc.ca//news/canada/newfoundland-labrador/tristen-keats-housing-crisis-shelters-1.7012896
https://www.cbc.ca//news/canada/toronto/peter-nygard-sexual-assault-trial-1.7015155
https://www.cbc.ca//news/canada/calgary/calgary-police-sgt-andrew-harnett-teen-identified-1.7013010
https://www.cbc.ca//news/canada/calgary/alberta-population-danielle-smith-high-speed-rail-analysis-1.7013363
https://www.cbc.ca//news/canada/calgary/alberta-wetland-oilsands-environment-1.7013213
https://www.cbc.ca//news/canada/ottawa/kingston-police-investigate-halloween-party-threats-to-jewish-community-1.7015146
https://www.cbc.ca//news/canada/toronto/ontario-doug-ford-greenbelt-land-development-housing-1.7014360
https://www.cbc.ca//news/canada/ottawa/teacher-suspension-locker-college-franco-ouest-1.7013016
https://www.cbc.ca//news/canada/manitoba/manitoba-carbon-tax-relief-1.7014540
https://www.cbc.ca//news/canada/british-columbia/ashley-simpson-murder-trial-begins-1.7011371
https://www.cbc.ca//news/canada/new-brunswick/doctors-college-physicians-surgeons-new-brunswick-foreign-trained-licence-1.7012884
https://www.cbc.ca//news/canada/prince-edward-island/pei-coastline-loss-2022-post-fiona-report-1.7010862
https://www.cbc.ca//news/canada/toronto/ontario-college-revokes-applications-again-1.7012594
https://www.cbc.ca//news/canada/saskatchewan/pelican-narrows-state-of-emergency-violence-1.7011085
https://www.cbc.ca//news/canada/manitoba/car-safety-inspection-canadian-tire-1.7009194
https://www.cbc.ca//news/canada/london/ontario-real-estate-farmland-agriculture-1.7012778
https://www.cbc.ca//news/canada/manitoba/grassroots-grannies-winnipeg-rap-song-1.7013093
https://www.cbc.ca//news/canada/british-columbia/b-c-therapists-lawsuit-mdma-trial-1.7010981
https://www.cbc.ca//news/canada/manitoba/silver-cross-mother-gloria-hooper-christopher-holopina-manitoba-1.7012544
```
By scrapping the html we see certain pattern on the html and this can make use as a filter with the code below
```
for i in soup.find_all('a', href=True):
    if 'news/canada/toronto' in i['href']:
        print('Toronto'+':'+'https://www.cbc.ca/'+i['href'])
    if 'news/canada/calgary' in i['href']:
        print('Calgary'+':'+'https://www.cbc.ca/'+i['href'])
    if 'news/canada/hamilton' in i['href']:
        print('Hamilton'+':'+'https://www.cbc.ca/'+i['href'])
    if 'news/canada/montreal' in i['href']:
        print('Montreal'+':'+'https://www.cbc.ca/'+i['href'])
```



###Improvement
To get a comprehensive search on keywods, we should include news that are not on the main page. Challenges come when there is no available element to select on the load more button. Perhaps Selenium could help with this situstion by acting as a human to click on thr load more button.


so getting the ntitle and only for toronto can be a good filter
