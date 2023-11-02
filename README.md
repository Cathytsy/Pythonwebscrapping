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
Detailed code and result can be referred to Python_Assignment2_Part1

## Second part - topic count - thanks to Will's code

Creating a function 
```
def topic_detection(sentence):
    Israel_Keywords =('Israel','Jewish','Jews')
    Palestine_Keywords =('Gaza','Palestine','Rafah','Muslim','Palestinian')
    Hamas_Keywords= ('Hamas')
    Israel = any(sentence.count(i)>0 for i in Israel_Keywords)
    Palestine = any(sentence.count(i)>0 for i in Palestine_Keywords)
    Hamas = any(sentence.count(i)>0 for i in Hamas_Keywords)
    topics = []
    if Israel == True:
        topics.append("Israel")
    if Palestine == True:
        topics.append("Palestine")
    if Hamas == True:
        topics.append('Hamas')
    return topics
```
Creating a dictionary
```
p_dictionary = {}
p_list = []
topic_list = []
for i in range(0, len(paragraphs)):
    if len(paragraphs[i].text) > 50:
      p_list.append(paragraphs[i].text)
      topic_list.append(topic_detection(paragraphs[i].text))
p_dictionary['Paragraphs'] = p_list
p_dictionary['Topics'] = topic_list
```

Separating the category and prepare for the bar chat
```
df['Israel'] = df['Topics'].apply(lambda x:1 if 'Israel' in x else 0
df['Palestine'] = df['Topics'].apply(lambda x:1 if 'Palestine' in x else 0)
df['Hamas'] = df['Topics'].apply(lambda x:1 if 'Hamas' in x else 0)
```
![image](https://github.com/Cathytsy/Pythonwebscrapping/assets/147212218/1461ee9a-b501-480c-bbcf-0867c9a8a5b6)

Setting y-axis and x-axis for the bar chat with the code below
```
topic_counts = [df['Palestine'].sum(), df['Israel'].sum(),df['Hamas'].sum()]
topic_labels = ['Palestine', 'Israel', 'Hamas']
```
### Result 
![image](https://github.com/Cathytsy/Pythonwebscrapping/assets/147212218/81ede47d-f726-4668-8b24-75c9b36db066)

An interesting finding is that Hamas - when putting only one keyword in that category - still result in the highest count. It seems like Hamas is a must put word when reporting about the war between Hamas and Israel. 


### Improvement
To get a comprehensive search on keywods, we should include news that are not on the main page. Challenges come when there is no available element to select on the load more button. Perhaps Selenium could help with this situstion by acting as a human to click on thr load more button.

