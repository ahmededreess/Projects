#### I use python to collect the data from Wuzzuf website about the data analyst job title. we collect ( job_title - company - location) for each job and extract them in csv file
libraries I used (Requests - BeautifulSoup - csv - ziplongest)
the code as the following :

```

import requests 
from bs4 import BeautifulSoup 
import csv
from itertools import zip_longest

job_title =[]
company = [] 
location = []

list = [1,2,3,4,5,6,7,8,9,10,11,12,13]
url_2 = []
url = 'https://wuzzuf.net/search/jobs/?q=data+analyst+&a=navbl'
request = requests.get(url)
source= request.content

soup = BeautifulSoup(source,'lxml') 

job_titles = soup.find_all('h2',{"class":"css-m604qf"})
companys = soup.find_all('a',{"class":"css-17s97q8"})
locations = soup.find_all('span',{"class":"css-5wys0k"})

for i in range(len(job_titles)):
    job_title.append(job_titles[i].text)
    company.append(companys[i].text)
    location.append(locations[i].text)
    
url_1 = 'https://wuzzuf.net/search/jobs/?a=navbl&q=data%20analyst%20&start='

for i in list :
    url_2.append(url_1 + str(i))
    

for x in url_2 :
    req = requests.get(x)
    sources = req.content
    soup_file = BeautifulSoup(sources,'lxml') 
    job_titles_p = soup_file.find_all('h2',{"class":"css-m604qf"})
    companys_p = soup_file.find_all('a',{"class":"css-17s97q8"})
    locations_p = soup_file.find_all('span',{"class":"css-5wys0k"})
    for z in range(len(job_titles_p)):
           job_title.append(job_titles_p[z].text)
           company.append(companys_p[z].text)
           location.append(locations_p[z].text)

file_4 = [job_title, company, location] 
exported = zip_longest(*file_4)
    
with open('D:\Software\Python\Projects\webscraping_3.csv','w') as myfile:
    wr = csv.writer(myfile)
    wr.writerow(["job_title","company","Location"])
    wr.writerows(exported)
    
```

