```python
import pandas as pd 
import re
```


```python
jobs = pd.read_csv(r"D:\Software\Python\Projects\Wuzzuf Project\archive (4)\Wuzzuf_Applications_Sample.csv")
```


```python
jobs.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1854190 entries, 0 to 1854189
    Data columns (total 4 columns):
     #   Column    Dtype 
    ---  ------    ----- 
     0   id        object
     1   user_id   object
     2   job_id    object
     3   app_date  object
    dtypes: object(4)
    memory usage: 56.6+ MB
    


```python
# insure there is not duplicate values in application_id 

jobs.nunique()
```




    id          1854190
    user_id      314460
    job_id        19208
    app_date    1824859
    dtype: int64




```python
# insure there is no null values in the data 

jobs.isnull().sum()
```




    id          0
    user_id     0
    job_id      0
    app_date    0
    dtype: int64




```python
# change the type of the column app_date

jobs.app_date = pd.to_datetime(jobs.app_date)
```


```python
job_posts = pd.read_csv(r"D:\Software\Python\Projects\Wuzzuf Project\archive (4)\Wuzzuf_Job_Posts_Sample.csv")
```


```python
job_posts.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 21850 entries, 0 to 21849
    Data columns (total 20 columns):
     #   Column            Non-Null Count  Dtype 
    ---  ------            --------------  ----- 
     0   id                21850 non-null  object
     1   city              21850 non-null  object
     2   job_title         21850 non-null  object
     3   job_category1     21850 non-null  object
     4   job_category2     21850 non-null  object
     5   job_category3     21850 non-null  object
     6   job_industry1     21850 non-null  object
     7   job_industry2     21850 non-null  object
     8   job_industry3     21850 non-null  object
     9   salary_minimum    21850 non-null  int64 
     10  salary_maximum    21850 non-null  int64 
     11  num_vacancies     21850 non-null  int64 
     12  career_level      21850 non-null  object
     13  experience_years  21850 non-null  object
     14  post_date         21850 non-null  object
     15  views             21850 non-null  int64 
     16  job_description   21576 non-null  object
     17  job_requirements  19217 non-null  object
     18  payment_period    21845 non-null  object
     19  currency          21845 non-null  object
    dtypes: int64(4), object(16)
    memory usage: 3.3+ MB
    


```python
job_posts.head(4)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>job_title</th>
      <th>job_category1</th>
      <th>job_category2</th>
      <th>job_category3</th>
      <th>job_industry1</th>
      <th>job_industry2</th>
      <th>job_industry3</th>
      <th>salary_minimum</th>
      <th>salary_maximum</th>
      <th>num_vacancies</th>
      <th>career_level</th>
      <th>experience_years</th>
      <th>post_date</th>
      <th>views</th>
      <th>job_description</th>
      <th>job_requirements</th>
      <th>payment_period</th>
      <th>currency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>516e4ed</td>
      <td>Ciro</td>
      <td>Sales &amp; Marketing Agent</td>
      <td>Sales/Retail/Business Development</td>
      <td>Marketing</td>
      <td>Select</td>
      <td>Telecommunications Services</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>3500</td>
      <td>8</td>
      <td>Entry Level</td>
      <td>0-1</td>
      <td>2014-01-01 06:01:41</td>
      <td>2602</td>
      <td>&lt;p&gt;&lt;strong&gt;Qualifications&lt;/strong&gt;:&lt;br /&gt;&lt;br /...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
    </tr>
    <tr>
      <th>1</th>
      <td>a361ef59</td>
      <td>Cairo</td>
      <td>German Training Coordinator</td>
      <td>Customer Service/Support</td>
      <td>Administration</td>
      <td>Human Resources</td>
      <td>Translation and Localization</td>
      <td>Business Services - Other</td>
      <td>Education</td>
      <td>1000</td>
      <td>5000</td>
      <td>8</td>
      <td>Entry Level</td>
      <td>0-2</td>
      <td>2014-01-01 20:01:18</td>
      <td>2213</td>
      <td>&lt;p&gt;&amp;bull;Placing jobs' ads on various websites...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7226ce78</td>
      <td>Cairo</td>
      <td>Junior Software Developer</td>
      <td>IT/Software Development</td>
      <td>Select</td>
      <td>Select</td>
      <td>Computer Software</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>2500</td>
      <td>1</td>
      <td>Entry Level</td>
      <td>2</td>
      <td>2014-01-02 11:01:03</td>
      <td>2940</td>
      <td>&lt;span style="text-decoration: underline;"&gt;&lt;str...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f4b2bcd6</td>
      <td>Cairo</td>
      <td>Application Support Engineer</td>
      <td>IT/Software Development</td>
      <td>Select</td>
      <td>Select</td>
      <td>Telecommunications Services</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>3500</td>
      <td>1</td>
      <td>Entry Level</td>
      <td>1-2</td>
      <td>2014-01-02 12:01:23</td>
      <td>2042</td>
      <td>&lt;strong&gt;&lt;span style="text-decoration: underlin...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
    </tr>
  </tbody>
</table>
</div>




```python
job_posts.dtypes
```




    id                  object
    city                object
    job_title           object
    job_category1       object
    job_category2       object
    job_category3       object
    job_industry1       object
    job_industry2       object
    job_industry3       object
    salary_minimum       int64
    salary_maximum       int64
    num_vacancies        int64
    career_level        object
    experience_years    object
    post_date           object
    views                int64
    job_description     object
    job_requirements    object
    payment_period      object
    currency            object
    dtype: object




```python
# change the datetype of the post_date to datetime

job_posts.post_date = pd.to_datetime(job_posts.post_date)
```


```python
# cleaning the column of ( city ) 
job_posts["city"].value_counts().head(60)
```




    Cairo                   14988
    Giza                     3886
    Alexandria                989
    6th of October             90
    Mansoura                   89
    New Cairo                  85
    10th of Ramadan            81
    Any                        60
    Giza / Cairo               45
    Ismailia                   39
    Cairo / Giza               36
    10th of ramadan city       36
    6th of October City        35
    Tanta                      32
    nasr city                  32
    menoufia                   31
    6 October                  30
    القاهرة                    29
    Suez                       26
    10th of Ramdan City        26
    6 of October               24
    Hurghada                   22
    maadi                      20
    Assuit                     20
    Obour city                 20
    Nasr City, Cairo           19
    cairo, giza                19
    all                        19
    Ismalia                    18
    Cairo/ Giza                16
    Egypt                      14
    Alexandria & Cairo         13
    alex                       12
    6th october                12
    Cairo & alexandria         12
    ASWAN                      12
    Qalyubia                   12
    Cairo - Giza               12
    Port Said                  12
    الجيزة                     11
    All over Egypt             11
    red sea                    11
    Giza/Cairo                 10
    Mohandessin                10
    Upper Egypt                10
    Sharkia                    10
    Cairo/Giza                 10
    Cairo & Giza               10
    Dakahlia                    9
    Cairo, Alexandria           9
    Alexandria/ Cairo           9
    Ciro                        8
    Sharm El Sheikh             8
    Alexandria / Cairo          8
    Asyut                       8
    cario                       8
    alexanderia                 7
    All Governorates            7
    Cairo - Alexandria          7
    Heliopolis                  7
    Name: city, dtype: int64




```python
# we will divide colums to (Caire - Gize & 6 October - ALex - Delta -  Others )
job_posts["city"] = job_posts["city"].str.strip()
def strip_city_name(x):
    if x[-1] ==',':
        x = x[:-1]
    return x.lower().replace('- ',',').replace('&',',').replace('/',',').replace('.','').replace(', ',',').replace(' ,',',').replace(' and ',',').replace('el ','').replace('el-','').strip()
job_posts['cleaned_city'] = job_posts.city.apply(lambda x : strip_city_name(x))

```


```python
job_posts["cleaned_city"].value_counts().head(60)
```




    cairo                   14989
    giza                     3886
    alexandria                989
    cairo,giza                108
    mansoura                   95
    6th of october             91
    new cairo                  85
    10th of ramadan            82
    giza,cairo                 70
    any                        60
    cairo,alexandria           40
    ismailia                   39
    alexandria,cairo           39
    10th of ramadan city       36
    6th of october city        35
    menoufia                   32
    tanta                      32
    nasr city                  32
    6 october                  30
    القاهرة                    29
    10th of ramdan city        26
    suez                       26
    6 of october               24
    obour city                 24
    hurghada                   22
    nasr city,cairo            21
    assuit                     20
    maadi                      20
    all                        19
    ismalia                    18
    egypt                      14
    port said                  12
    alex                       12
    aswan                      12
    6th october                12
    qalyubia                   12
    الجيزة                     11
    obour                      11
    all over egypt             11
    sharkia                    11
    red sea                    11
    mohandessin                10
    upper egypt                10
    gharbia                    10
    sharm sheikh               10
    dakahlia                    9
    kafr sheikh                 8
    cario                       8
    cairo,alex                  8
    ciro                        8
    asyut                       8
    al sharqia                  7
    damietta                    7
    heliopolis                  7
    all governorates            7
    alexanderia                 7
    remote                      7
    delta                       6
    minia                       6
    sohag                       6
    Name: cleaned_city, dtype: int64




```python
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['New Cairo','nasr city ','القاهرة','maadi','Nasr City, Cairo',
                                'Qalyubia','Ciro','nasr city','cairo','new cairo','nasr city,cairo',"qalyubia",'mohandessin','القاهره','cario'
                                        ,'ciro','heliopolis','cairo,egypt','heliopolis,cairo','mohandeseen','cairo,new cairo'
                                                ], 'Cairo'))
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['giza','6th of october','cairo,giza','giza,cairo'
                                                        ,'6th october','6th of october city','6 october','6 of october'
                                                        ,'الجيزة','6 october city','6 of october city'],'Giza & 6 October'))
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['10th of ramadan','10th of ramadan city'
                                                    ,'10th of ramdan city','obour city','sharkia',"obour",'al sharqia','sharqia'],'10th of Ramadan'))
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['alexandria','cairo,alexandria','alexandria,cairo'
                                                        ,'alex','cairo,alex','alexanderia'],'Alexandria'))
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['mansoura','tanta','menoufia','gharbia','dakahlia','kafr sheikh'
                                                               ,'monufia','delta','monofeya','menoufya' ],'Delta'))
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['assuit','aswan','upper egypt','asyut','minia'
                                                        ,'assiut','fayoum','qena','sohag','minya'
                                                        ],'Upper_Egypt'))
job_posts["cleaned_city"] = job_posts["cleaned_city"].replace(dict.fromkeys(['any','red sea','ismailia','suez','hurghada','ismalia',"port said"
                                                                            ,"sharm sheikh",'damietta','portsaid'],'Other'))
```


```python
job_posts["cleaned_city"].value_counts().head(60)
```




    Cairo                                                                              15246
    Giza & 6 October                                                                    4275
    Alexandria                                                                          1095
    Other                                                                                210
    Delta                                                                                207
    10th of Ramadan                                                                      202
    Upper_Egypt                                                                           83
    all                                                                                   19
    egypt                                                                                 14
    all over egypt                                                                        11
    all governorates                                                                       7
    remote                                                                                 7
    cairo,alex,mansoura                                                                    5
    south sinai                                                                            4
    all around egypt                                                                       4
    october                                                                                4
    sharkia,behira,menoufia,qalyubiyah,canal,faioum giza,minya,asyut,bani suif,qena        4
    sadat city                                                                             4
    cairo,nasr city                                                                        4
    beni suef                                                                              4
    dokki                                                                                  4
    zagazig                                                                                4
    ain sokhna                                                                             4
    giza,egypt                                                                             4
    helwan                                                                                 4
    alexandria,beheira                                                                     4
    maadi,cairo                                                                            4
    anywhere                                                                               3
    mohndseen                                                                              3
    sheikh zayed                                                                           3
    october city                                                                           3
    6th october city                                                                       3
    port said,ismailia,damitta,mansoura,suez                                               3
    marsa alam                                                                             3
    qalyoubia                                                                              3
    cairo,alexanderia                                                                      3
    cairo,mansoura                                                                         3
    upper egypt,red sea                                                                    3
    cairo,giza,alexandria                                                                  3
    greater cairo                                                                          3
    remote (work from home)                                                                3
    giza,6th of october                                                                    3
    giza,alexandria                                                                        3
    mounfia                                                                                3
    ciaro                                                                                  3
    beheira                                                                                3
    menofia                                                                                3
    الاسكندرية                                                                             3
    beni suef,ismailia,mansoura,cairo                                                      3
    beni suef,ismailia,mansoura                                                            3
    مدينة 6 اكتوير                                                                         3
    bani suef,ismailia,mansoura,cairo                                                      3
    obour industrial city                                                                  2
    caito                                                                                  2
    greater cairo,mansoura                                                                 2
    gizah                                                                                  2
    all provenience                                                                        2
    cairo-giza                                                                             2
    east owainat                                                                           2
    shrouk                                                                                 2
    Name: cleaned_city, dtype: int64




```python
job_posts["cleaned_city"].nunique()
```




    345




```python
# cleaning the column ( job title )
job_posts["job_title"].value_counts().head(30)
```




    Graphic Designer                   363
    Call Center Agent                  204
    Social Media Specialist            178
    Sales Engineer                     171
    Web Developer                      146
    Marketing Specialist               130
    Customer Service Representative    129
    PHP Developer                      127
    Web Designer                       127
    Customer Service Agent             116
    Telesales Agent                    110
    Senior PHP Developer                94
    Software Developer                  93
    Marketing Manager                   92
    Marketing Executive                 91
    Java Developer                      86
    iOS Developer                       79
    Graphic Designer                    77
    Digital Marketing Specialist        77
    Senior .Net Developer               75
    Android Developer                   73
    Project Manager                     69
    Senior Java Developer               69
    IT Specialist                       66
    Senior Software Developer           63
    Sales Account Manager               61
    .Net Developer                      60
    Interior Designer                   60
    Mechanical Engineer                 58
    Site Engineer                       56
    Name: job_title, dtype: int64




```python
job_posts["job_title"] = job_posts["job_title"].str.strip()
```


```python
job_posts["job_title"].value_counts()
```




    Graphic Designer                        445
    Call Center Agent                       250
    Social Media Specialist                 224
    Sales Engineer                          213
    Customer Service Representative         178
                                           ... 
    Call Center Team Manager - Italian        1
    Senior web Designer                       1
    Senior Planning Engineer- Alexandria      1
    Senior .NET/ SharePoint Developer         1
    Microsoft Product Manager                 1
    Name: job_title, Length: 9611, dtype: int64




```python
job_posts["experience_years"].unique()[::-10]
```




    array(['3 :5', '6 - 8 ', '4  to  6', '3--5', '3-7 ', '2 at least',
           '3year', '1 - 2 years',
           '3-5 Years of experience in Cement Industry', '5 ', '5-6 ', '1- 5',
           '<8', '0 or 1 year', '7  to 9 ', '6+ ', '0 - 5', '6-12', '9-15',
           '5-8 ', ' 2+', '0,1', '4 years', '3 سنوات على الاقل', '0 - 1',
           '1-7', '8 -10', '0 - 3', '5 -7 ', '1- 3', '4-5 ', '2:4', '0 -2',
           '1~2', '1-5 ', '2-3 ', '2:3', '7 - 15', '1-3 ', '20-25', '1-4 ',
           '0-1 ', '7-10', '0 , 1 or 2', '3-5', '6', '0-1'], dtype=object)




```python
l = job_posts.experience_years.values
range_exps =[]
for i in l:
    pattern =re.findall('\\b\\d+\\b', i)
    if len(pattern) == 0:
        #min_exp = min([int(s) for s in i if s.isdigit()])
        max_exp = max([int(s) for s in i if s.isdigit()])
    else:
        #min_exp = min([int(s) for s in pattern])
        max_exp = max([int(s) for s in pattern])
    # check for the + sign
    if '+' in i:
        max_exp += 3
    # add to our cleaned list
    if max_exp == 0 :
        range_exp = 'Fresh'
    elif 0 < max_exp <= 5:
        range_exp = 'below 5'
    elif 5 < max_exp <= 10:
        range_exp = '5-10'
    elif 10 < max_exp <= 15:
        range_exp = '10-15'
    elif 15 < max_exp <= 20:
        range_exp = '15-20'
    elif 20 < max_exp <= 25:
        range_exp = '20-25'
    else:
        range_exp = 'above 25'
    range_exps.append(range_exp)
```


```python
job_posts['experience_range'] = range_exps
```


```python
job_posts.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>job_title</th>
      <th>job_category1</th>
      <th>job_category2</th>
      <th>job_category3</th>
      <th>job_industry1</th>
      <th>job_industry2</th>
      <th>job_industry3</th>
      <th>salary_minimum</th>
      <th>...</th>
      <th>career_level</th>
      <th>experience_years</th>
      <th>post_date</th>
      <th>views</th>
      <th>job_description</th>
      <th>job_requirements</th>
      <th>payment_period</th>
      <th>currency</th>
      <th>cleaned_city</th>
      <th>experience_range</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>516e4ed</td>
      <td>Ciro</td>
      <td>Sales &amp; Marketing Agent</td>
      <td>Sales/Retail/Business Development</td>
      <td>Marketing</td>
      <td>Select</td>
      <td>Telecommunications Services</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>...</td>
      <td>Entry Level</td>
      <td>0-1</td>
      <td>2014-01-01 06:01:41</td>
      <td>2602</td>
      <td>&lt;p&gt;&lt;strong&gt;Qualifications&lt;/strong&gt;:&lt;br /&gt;&lt;br /...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Cairo</td>
      <td>below 5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>a361ef59</td>
      <td>Cairo</td>
      <td>German Training Coordinator</td>
      <td>Customer Service/Support</td>
      <td>Administration</td>
      <td>Human Resources</td>
      <td>Translation and Localization</td>
      <td>Business Services - Other</td>
      <td>Education</td>
      <td>1000</td>
      <td>...</td>
      <td>Entry Level</td>
      <td>0-2</td>
      <td>2014-01-01 20:01:18</td>
      <td>2213</td>
      <td>&lt;p&gt;&amp;bull;Placing jobs' ads on various websites...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Cairo</td>
      <td>below 5</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 22 columns</p>
</div>




```python
jobs.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1854190 entries, 0 to 1854189
    Data columns (total 4 columns):
     #   Column    Dtype         
    ---  ------    -----         
     0   id        object        
     1   user_id   object        
     2   job_id    object        
     3   app_date  datetime64[ns]
    dtypes: datetime64[ns](1), object(3)
    memory usage: 56.6+ MB
    


```python
# start analysis 
# Analysis the number 
# 1- No of applications ,  , 

jobs["id"].nunique()
```




    1854190




```python
# No of applicants
jobs["user_id"].nunique()
```




    314460




```python
# No of jobs
jobs["job_id"].nunique()
```




    19208




```python
# No of applications (year - month - day - dayname ) 

jobs["year"] = jobs["app_date"].dt.year
jobs["Month"] = jobs["app_date"].dt.month
jobs["day"] = jobs["app_date"].dt.day
jobs["day_name"] = jobs["app_date"].dt.day_name()

app_year = jobs.groupby("year", as_index = False)["id"].count()
app_Month = jobs.groupby("Month", as_index = False)["id"].count()
app_day = jobs.groupby("day", as_index = False)["id"].count()
app_day_name = jobs.groupby("day_name", as_index = False)["id"].count()
```


```python
# No of jobs ( Year - Month )
job_posts["year"] = job_posts["post_date"].dt.year
job_posts["month"] = job_posts["post_date"].dt.month

jobs_year = job_posts.groupby('year', as_index = False)["id"].count()
jobs_month = job_posts.groupby('month', as_index = False)["id"].count()
```


```python
# Job titles most demended 

job_posts["job_title"].value_counts().head(50)
```




    Graphic Designer                   445
    Call Center Agent                  250
    Social Media Specialist            224
    Sales Engineer                     213
    Customer Service Representative    178
    Web Developer                      165
    Marketing Specialist               162
    Telesales Agent                    157
    Web Designer                       156
    PHP Developer                      151
    Customer Service Agent             150
    Marketing Executive                117
    Marketing Manager                  117
    Software Developer                 111
    Senior PHP Developer               108
    Digital Marketing Specialist       107
    Java Developer                      99
    iOS Developer                       90
    Senior .Net Developer               88
    Senior Java Developer               85
    Senior Software Developer           84
    Android Developer                   83
    Project Manager                     81
    Mechanical Engineer                 76
    Architect                           76
    IT Specialist                       76
    E-Marketing Specialist              72
    Interior Designer                   70
    .Net Developer                      69
    Sales Account Manager               68
    Site Engineer                       68
    Project Coordinator                 62
    Account Manager                     61
    Senior Android Developer            61
    Technical Office Engineer           61
    Senior Graphic Designer             54
    Telesales Representative            54
    Electrical Engineer                 53
    Senior iOS Developer                50
    Senior Web Developer                50
    Marketing Coordinator               49
    Civil Engineer                      48
    Maintenance Engineer                48
    Receptionist                        48
    Production Engineer                 48
    Technical Sales Engineer            45
    Social Media Executive              45
    Customer Care Agent                 44
    Sales Representative                43
    Software Engineer                   43
    Name: job_title, dtype: int64




```python
job_posts["job_category1"].value_counts()
```




    IT/Software Development                 5762
    Engineering                             3405
    Customer Service/Support                2917
    Marketing                               2590
    Sales/Retail/Business Development       1998
    Creative/Design                         1712
    Quality Assurance/Quality Control        474
    Administration                           459
    Editorial/Writing                        458
    Installation/Maintenance/Repair          287
    Project/Program Management               268
    Education/Training                       224
    Manufacturing/Production/Operations      184
    Medical                                  131
    Media/Journalism/Publishing              112
    Pharmaceutical                           107
    Biotech/R&D/Science                      106
    Tourism/Travel                            89
    Research                                  85
    Logistics/Transportation                  74
    Building Construction/Skilled Trades      71
    Management                                70
    Business                                  62
    Accounting/Finance/Insurance              56
    Food Services/Hospitality                 53
    Banking                                   42
    Human Resources                           28
    Sports and Leisure                        11
    Fashion                                   11
    Legal                                      4
    Name: job_category1, dtype: int64




```python
job_posts["job_industry1"].value_counts().head(10)
```




    Computer Software                            2824
    Information Technology Services              2029
    Advertising and PR Services                  1108
    Telecommunications Services                  1021
    Computer/IT Services                          976
    Marketing and Advertising                     795
    Consumer Services                             727
    Engineering Services                          692
    Education                                     634
    Engineering - Mechanical or Industrial        616
    Name: job_industry1, dtype: int64




```python
job_posts.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>job_title</th>
      <th>job_category1</th>
      <th>job_category2</th>
      <th>job_category3</th>
      <th>job_industry1</th>
      <th>job_industry2</th>
      <th>job_industry3</th>
      <th>salary_minimum</th>
      <th>...</th>
      <th>post_date</th>
      <th>views</th>
      <th>job_description</th>
      <th>job_requirements</th>
      <th>payment_period</th>
      <th>currency</th>
      <th>cleaned_city</th>
      <th>experience_range</th>
      <th>year</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>516e4ed</td>
      <td>Ciro</td>
      <td>Sales &amp; Marketing Agent</td>
      <td>Sales/Retail/Business Development</td>
      <td>Marketing</td>
      <td>Select</td>
      <td>Telecommunications Services</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>...</td>
      <td>2014-01-01 06:01:41</td>
      <td>2602</td>
      <td>&lt;p&gt;&lt;strong&gt;Qualifications&lt;/strong&gt;:&lt;br /&gt;&lt;br /...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Cairo</td>
      <td>below 5</td>
      <td>2014</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>a361ef59</td>
      <td>Cairo</td>
      <td>German Training Coordinator</td>
      <td>Customer Service/Support</td>
      <td>Administration</td>
      <td>Human Resources</td>
      <td>Translation and Localization</td>
      <td>Business Services - Other</td>
      <td>Education</td>
      <td>1000</td>
      <td>...</td>
      <td>2014-01-01 20:01:18</td>
      <td>2213</td>
      <td>&lt;p&gt;&amp;bull;Placing jobs' ads on various websites...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Cairo</td>
      <td>below 5</td>
      <td>2014</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7226ce78</td>
      <td>Cairo</td>
      <td>Junior Software Developer</td>
      <td>IT/Software Development</td>
      <td>Select</td>
      <td>Select</td>
      <td>Computer Software</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>...</td>
      <td>2014-01-02 11:01:03</td>
      <td>2940</td>
      <td>&lt;span style="text-decoration: underline;"&gt;&lt;str...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Cairo</td>
      <td>below 5</td>
      <td>2014</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f4b2bcd6</td>
      <td>Cairo</td>
      <td>Application Support Engineer</td>
      <td>IT/Software Development</td>
      <td>Select</td>
      <td>Select</td>
      <td>Telecommunications Services</td>
      <td>Select</td>
      <td>Select</td>
      <td>2000</td>
      <td>...</td>
      <td>2014-01-02 12:01:23</td>
      <td>2042</td>
      <td>&lt;strong&gt;&lt;span style="text-decoration: underlin...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Cairo</td>
      <td>below 5</td>
      <td>2014</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3fee6f73</td>
      <td>Alexandria</td>
      <td>Electrical Maintenance Engineer</td>
      <td>Engineering</td>
      <td>Select</td>
      <td>Select</td>
      <td>Food and Beverage Production</td>
      <td>Select</td>
      <td>Select</td>
      <td>5000</td>
      <td>...</td>
      <td>2014-01-21 13:45:56</td>
      <td>5684</td>
      <td>Job Title: Electrical Maintenance Engineer&lt;br ...</td>
      <td>NaN</td>
      <td>Per Month</td>
      <td>Egyptian Pound</td>
      <td>Alexandria</td>
      <td>below 5</td>
      <td>2014</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>




```python
job_posts["experience_range"].value_counts()
```




    below 5     15675
    5-10         5271
    10-15         570
    Fresh         206
    15-20         107
    20-25          19
    above 25        2
    Name: experience_range, dtype: int64




```python
views_of_jobs = job_posts.groupby("job_title", as_index = False)["views"].sum().sort_values(by = "views",ascending = False)
```


```python
file_1 = pd.ExcelWriter("Excel.xlsx")
views_of_jobs.to_excel(file_1, sheet_name = "jobs_views")
jobs_year.to_excel(file_1 , sheet_name = "jobs_year")
jobs_month.to_excel(file_1 , sheet_name = "jobs_month")
app_year.to_excel(file_1 , sheet_name = "app_year")
app_Month.to_excel(file_1 , sheet_name = "app_month")
app_day.to_excel(file_1 , sheet_name = "app_day")
app_day_name.to_excel(file_1 , sheet_name = "app_day_name")
file_1.close()

```


```python

```
