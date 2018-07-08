

```python
import pandas as pd
import pymysql
```


```python
# 连接本地的mysql数据库，使用sakila库，sakila库是mysql的示例库，一般都有，如果没有的可以安装
# 如果不实际运行sql和pandans语句可以不执行这段代码
conn = pymysql.connect(host="127.0.0.1", user="root", passwd="root", db="sakila", port=3306, charset='utf8')
film_df = pd.read_sql("select * from film;", conn)
film_actor_df = pd.read_sql("select * from film_actor;", conn)
actor_df = pd.read_sql("select * from actor;", conn)

```

# desc
*注：一般每个code cell里前面的是sql语句（用#注释起来了），随后是对应的pandas语句*


```python
# desc film;
film_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000 entries, 0 to 999
    Data columns (total 13 columns):
    film_id                 1000 non-null int64
    title                   1000 non-null object
    description             1000 non-null object
    release_year            1000 non-null int64
    language_id             1000 non-null int64
    original_language_id    0 non-null object
    rental_duration         1000 non-null int64
    rental_rate             1000 non-null float64
    length                  1000 non-null int64
    replacement_cost        1000 non-null float64
    rating                  1000 non-null object
    special_features        1000 non-null object
    last_update             1000 non-null datetime64[ns]
    dtypes: datetime64[ns](1), float64(2), int64(5), object(5)
    memory usage: 101.6+ KB
    


```python
film_df.columns.values
```




    array(['film_id', 'title', 'description', 'release_year', 'language_id',
           'original_language_id', 'rental_duration', 'rental_rate', 'length',
           'replacement_cost', 'rating', 'special_features', 'last_update'], dtype=object)




```python
film_df.shape
```




    (1000, 13)



# select 


```python
# select * 
# from film;
film_df
```


```python
# select * 
# from film 
# limit 3;
film_df.head(3) # we can also transepose the result by add .T, film_df.head(3).T
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
      <th>release_year</th>
      <th>language_id</th>
      <th>original_language_id</th>
      <th>rental_duration</th>
      <th>rental_rate</th>
      <th>length</th>
      <th>replacement_cost</th>
      <th>rating</th>
      <th>special_features</th>
      <th>last_update</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ACADEMY DINOSAUR</td>
      <td>A Epic Drama of a Feminist And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>86</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>ACE GOLDFINGER</td>
      <td>A Astounding Epistle of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>48</td>
      <td>12.99</td>
      <td>G</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ADAPTATION HOLES</td>
      <td>A Astounding Reflection of a Lumberjack And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
  </tbody>
</table>
</div>




```python
# select film_id, title, description
# from film
# limit 10, 5;
film_df[["film_id", "title", "description"]][10:10 + 5] # in sql 10 is offset, 5 is limit, so [10:10 + 5] in pandas
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>ALAMO VIDEOTAPE</td>
      <td>A Boring Epistle of a Butler And a Cat who mus...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>ALASKA PHANTOM</td>
      <td>A Fanciful Saga of a Hunter And a Pastry Chef ...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>ALI FOREVER</td>
      <td>A Action-Packed Drama of a Dentist And a Croco...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>ALICE FANTASIA</td>
      <td>A Emotional Drama of a A Shark And a Database ...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15</td>
      <td>ALIEN CENTER</td>
      <td>A Brilliant Drama of a Cat And a Mad Scientist...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# select distinct rating
# from film;

film_df['rating'].unique()
```




    array(['PG', 'G', 'NC-17', 'PG-13', 'R'], dtype=object)



# count


```python
# select count(*)
# from film;

len(film_df)
```




    1000




```python
# select count(distinct rating)
# from film;

len(film_df['rating'].unique())
```




    5



# where


```python
# select film_id, title, description
# from film
# where film_id = 10;

film_df[film_df["film_id"] == 10][["film_id", "title", "description"]]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>ALADDIN CALENDAR</td>
      <td>A Action-Packed Tale of a Man And a Lumberjack...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# select film_id, title, description
# from film
# where rental_rate > 2
#       and length < 120;

film_df[(film_df["rental_rate"] > 2) & (film_df["length"] < 120)][["film_id", "title", "description"]]
```

# in


```python
# SELECT  * 
# FROM sakila.film
# where rental_duration in (3,6,7);

film_df[film_df.rental_duration.isin([3,6,7])]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
      <th>release_year</th>
      <th>language_id</th>
      <th>original_language_id</th>
      <th>rental_duration</th>
      <th>rental_rate</th>
      <th>length</th>
      <th>replacement_cost</th>
      <th>rating</th>
      <th>special_features</th>
      <th>last_update</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ACADEMY DINOSAUR</td>
      <td>A Epic Drama of a Feminist And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>86</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>ACE GOLDFINGER</td>
      <td>A Astounding Epistle of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>48</td>
      <td>12.99</td>
      <td>G</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ADAPTATION HOLES</td>
      <td>A Astounding Reflection of a Lumberjack And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>AFRICAN EGG</td>
      <td>A Fast-Paced Documentary of a Pastry Chef And ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>130</td>
      <td>22.99</td>
      <td>G</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>AGENT TRUMAN</td>
      <td>A Intrepid Panorama of a Robot And a Boy who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>169</td>
      <td>17.99</td>
      <td>PG</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>AIRPLANE SIERRA</td>
      <td>A Touching Saga of a Hunter And a Butler who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>62</td>
      <td>28.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>AIRPORT POLLOCK</td>
      <td>A Epic Tale of a Moose And a Girl who must Con...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>54</td>
      <td>15.99</td>
      <td>R</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>ALABAMA DEVIL</td>
      <td>A Thoughtful Panorama of a Database Administra...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>114</td>
      <td>21.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>ALADDIN CALENDAR</td>
      <td>A Action-Packed Tale of a Man And a Lumberjack...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>63</td>
      <td>24.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>ALAMO VIDEOTAPE</td>
      <td>A Boring Epistle of a Butler And a Cat who mus...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>126</td>
      <td>16.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>ALASKA PHANTOM</td>
      <td>A Fanciful Saga of a Hunter And a Pastry Chef ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>136</td>
      <td>22.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>ALICE FANTASIA</td>
      <td>A Emotional Drama of a A Shark And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>94</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>ALLEY EVOLUTION</td>
      <td>A Fast-Paced Drama of a Robot And a Composer w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>180</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>ALONE TRIP</td>
      <td>A Fast-Paced Character Study of a Composer And...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>82</td>
      <td>14.99</td>
      <td>R</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>ALTER VICTORY</td>
      <td>A Thoughtful Drama of a Composer And a Feminis...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>57</td>
      <td>27.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>AMADEUS HOLY</td>
      <td>A Emotional Display of a Pioneer And a Technic...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>113</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>AMERICAN CIRCUS</td>
      <td>A Insightful Drama of a Girl And a Astronaut w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>129</td>
      <td>17.99</td>
      <td>R</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>21</th>
      <td>22</td>
      <td>AMISTAD MIDSUMMER</td>
      <td>A Emotional Character Study of a Dentist And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>85</td>
      <td>10.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>22</th>
      <td>23</td>
      <td>ANACONDA CONFESSIONS</td>
      <td>A Lacklusture Display of a Dentist And a Denti...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>92</td>
      <td>9.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>23</th>
      <td>24</td>
      <td>ANALYZE HOOSIERS</td>
      <td>A Thoughtful Display of a Explorer And a Pastr...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>181</td>
      <td>19.99</td>
      <td>R</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>24</th>
      <td>25</td>
      <td>ANGELS LIFE</td>
      <td>A Thoughtful Display of a Woman And a Astronau...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>74</td>
      <td>15.99</td>
      <td>G</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>ANNIE IDENTITY</td>
      <td>A Amazing Panorama of a Pastry Chef And a Boat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>86</td>
      <td>15.99</td>
      <td>G</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>26</th>
      <td>27</td>
      <td>ANONYMOUS HUMAN</td>
      <td>A Amazing Reflection of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>0.99</td>
      <td>179</td>
      <td>12.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>31</th>
      <td>32</td>
      <td>APOCALYPSE FLAMINGOS</td>
      <td>A Astounding Story of a Dog And a Squirrel who...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>119</td>
      <td>11.99</td>
      <td>R</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>33</th>
      <td>34</td>
      <td>ARABIA DOGMA</td>
      <td>A Touching Epistle of a Madman And a Mad Cow w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>62</td>
      <td>29.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>35</th>
      <td>36</td>
      <td>ARGONAUTS TOWN</td>
      <td>A Emotional Epistle of a Forensic Psychologist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>0.99</td>
      <td>127</td>
      <td>12.99</td>
      <td>PG-13</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>36</th>
      <td>37</td>
      <td>ARIZONA BANG</td>
      <td>A Brilliant Panorama of a Mad Scientist And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>121</td>
      <td>28.99</td>
      <td>PG</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>37</th>
      <td>38</td>
      <td>ARK RIDGEMONT</td>
      <td>A Beautiful Yarn of a Pioneer And a Monkey who...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>68</td>
      <td>25.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>42</th>
      <td>43</td>
      <td>ATLANTIS CAUSE</td>
      <td>A Thrilling Yarn of a Feminist And a Hunter wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>170</td>
      <td>15.99</td>
      <td>G</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>45</th>
      <td>46</td>
      <td>AUTUMN CROW</td>
      <td>A Beautiful Tale of a Dentist And a Mad Cow wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>108</td>
      <td>13.99</td>
      <td>G</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>953</th>
      <td>954</td>
      <td>WAKE JAWS</td>
      <td>A Beautiful Saga of a Feminist And a Composer ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>4.99</td>
      <td>73</td>
      <td>18.99</td>
      <td>G</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>954</th>
      <td>955</td>
      <td>WALLS ARTIST</td>
      <td>A Insightful Panorama of a Teacher And a Teach...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>4.99</td>
      <td>135</td>
      <td>19.99</td>
      <td>PG</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>955</th>
      <td>956</td>
      <td>WANDA CHAMBER</td>
      <td>A Insightful Drama of a A Shark And a Pioneer ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>4.99</td>
      <td>107</td>
      <td>23.99</td>
      <td>PG-13</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>956</th>
      <td>957</td>
      <td>WAR NOTTING</td>
      <td>A Boring Drama of a Teacher And a Sumo Wrestle...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>4.99</td>
      <td>80</td>
      <td>26.99</td>
      <td>G</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>957</th>
      <td>958</td>
      <td>WARDROBE PHANTOM</td>
      <td>A Action-Packed Display of a Mad Cow And a Ast...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>178</td>
      <td>19.99</td>
      <td>G</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>958</th>
      <td>959</td>
      <td>WARLOCK WEREWOLF</td>
      <td>A Astounding Yarn of a Pioneer And a Crocodile...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>83</td>
      <td>10.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>960</th>
      <td>961</td>
      <td>WASH HEAVENLY</td>
      <td>A Awe-Inspiring Reflection of a Cat And a Pion...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>4.99</td>
      <td>161</td>
      <td>22.99</td>
      <td>R</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>961</th>
      <td>962</td>
      <td>WASTELAND DIVINE</td>
      <td>A Fanciful Story of a Database Administrator A...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>85</td>
      <td>18.99</td>
      <td>PG</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>964</th>
      <td>965</td>
      <td>WATERSHIP FRONTIER</td>
      <td>A Emotional Yarn of a Boat And a Crocodile who...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>112</td>
      <td>28.99</td>
      <td>G</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>965</th>
      <td>966</td>
      <td>WEDDING APOLLO</td>
      <td>A Action-Packed Tale of a Student And a Waitre...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>70</td>
      <td>14.99</td>
      <td>PG</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>967</th>
      <td>968</td>
      <td>WEREWOLF LOLA</td>
      <td>A Fanciful Story of a Man And a Sumo Wrestler ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>79</td>
      <td>19.99</td>
      <td>G</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>969</th>
      <td>970</td>
      <td>WESTWARD SEABISCUIT</td>
      <td>A Lacklusture Tale of a Butler And a Husband w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>0.99</td>
      <td>52</td>
      <td>11.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>972</th>
      <td>973</td>
      <td>WIFE TURN</td>
      <td>A Awe-Inspiring Epistle of a Teacher And a Fem...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>183</td>
      <td>27.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>974</th>
      <td>975</td>
      <td>WILLOW TRACY</td>
      <td>A Brilliant Panorama of a Boat And a Astronaut...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>137</td>
      <td>22.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>975</th>
      <td>976</td>
      <td>WIND PHANTOM</td>
      <td>A Touching Saga of a Madman And a Forensic Psy...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>111</td>
      <td>12.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>976</th>
      <td>977</td>
      <td>WINDOW SIDE</td>
      <td>A Astounding Character Study of a Womanizer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>85</td>
      <td>25.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>977</th>
      <td>978</td>
      <td>WISDOM WORKER</td>
      <td>A Unbelieveable Saga of a Forensic Psychologis...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>98</td>
      <td>12.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>978</th>
      <td>979</td>
      <td>WITCHES PANIC</td>
      <td>A Awe-Inspiring Drama of a Secret Agent And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>100</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>980</th>
      <td>981</td>
      <td>WOLVES DESIRE</td>
      <td>A Fast-Paced Drama of a Squirrel And a Robot w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>0.99</td>
      <td>55</td>
      <td>13.99</td>
      <td>NC-17</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>982</th>
      <td>983</td>
      <td>WON DARES</td>
      <td>A Unbelieveable Documentary of a Teacher And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>105</td>
      <td>18.99</td>
      <td>PG</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>983</th>
      <td>984</td>
      <td>WONDERFUL DROP</td>
      <td>A Boring Panorama of a Woman And a Madman who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>126</td>
      <td>20.99</td>
      <td>NC-17</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>985</th>
      <td>986</td>
      <td>WONKA SEA</td>
      <td>A Brilliant Saga of a Boat And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>85</td>
      <td>24.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>986</th>
      <td>987</td>
      <td>WORDS HUNTER</td>
      <td>A Action-Packed Reflection of a Composer And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>116</td>
      <td>13.99</td>
      <td>PG</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>987</th>
      <td>988</td>
      <td>WORKER TARZAN</td>
      <td>A Action-Packed Yarn of a Secret Agent And a T...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>139</td>
      <td>26.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>989</th>
      <td>990</td>
      <td>WORLD LEATHERNECKS</td>
      <td>A Unbelieveable Tale of a Pioneer And a Astron...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>171</td>
      <td>13.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>992</th>
      <td>993</td>
      <td>WRONG BEHAVIOR</td>
      <td>A Emotional Saga of a Crocodile And a Sumo Wre...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>178</td>
      <td>10.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>993</th>
      <td>994</td>
      <td>WYOMING STORM</td>
      <td>A Awe-Inspiring Panorama of a Robot And a Boat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>100</td>
      <td>29.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>995</th>
      <td>996</td>
      <td>YOUNG LANGUAGE</td>
      <td>A Unbelieveable Yarn of a Boat And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>183</td>
      <td>9.99</td>
      <td>G</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>997</th>
      <td>998</td>
      <td>ZHIVAGO CORE</td>
      <td>A Fateful Yarn of a Composer And a Man who mus...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>105</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>999</th>
      <td>1000</td>
      <td>ZORRO ARK</td>
      <td>A Intrepid Panorama of a Mad Scientist And a B...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
  </tbody>
</table>
<p>606 rows × 13 columns</p>
</div>




```python
# SELECT  * 
# FROM sakila.film
# where rental_duration not in (3,6,7);

film_df[~film_df.rental_duration.isin([3,6,7])]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
      <th>release_year</th>
      <th>language_id</th>
      <th>original_language_id</th>
      <th>rental_duration</th>
      <th>rental_rate</th>
      <th>length</th>
      <th>replacement_cost</th>
      <th>rating</th>
      <th>special_features</th>
      <th>last_update</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>AFFAIR PREJUDICE</td>
      <td>A Fanciful Documentary of a Frisbee And a Lumb...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>117</td>
      <td>26.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>ALI FOREVER</td>
      <td>A Action-Packed Drama of a Dentist And a Croco...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>150</td>
      <td>21.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15</td>
      <td>ALIEN CENTER</td>
      <td>A Brilliant Drama of a Cat And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>46</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>AMELIE HELLFIGHTERS</td>
      <td>A Boring Drama of a Woman And a Squirrel who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>79</td>
      <td>23.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>27</th>
      <td>28</td>
      <td>ANTHEM LUKE</td>
      <td>A Touching Panorama of a Waitress And a Woman ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>91</td>
      <td>16.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29</td>
      <td>ANTITRUST TOMATOES</td>
      <td>A Fateful Yarn of a Womanizer And a Feminist w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>168</td>
      <td>11.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>29</th>
      <td>30</td>
      <td>ANYTHING SAVANNAH</td>
      <td>A Epic Story of a Pastry Chef And a Woman who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>82</td>
      <td>27.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>30</th>
      <td>31</td>
      <td>APACHE DIVINE</td>
      <td>A Awe-Inspiring Reflection of a Pastry Chef An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>92</td>
      <td>16.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>32</th>
      <td>33</td>
      <td>APOLLO TEEN</td>
      <td>A Action-Packed Reflection of a Crocodile And ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>153</td>
      <td>15.99</td>
      <td>PG-13</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>34</th>
      <td>35</td>
      <td>ARACHNOPHOBIA ROLLERCOASTER</td>
      <td>A Action-Packed Reflection of a Pastry Chef An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>147</td>
      <td>24.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>38</th>
      <td>39</td>
      <td>ARMAGEDDON LOST</td>
      <td>A Fast-Paced Tale of a Boat And a Teacher who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>99</td>
      <td>10.99</td>
      <td>G</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>39</th>
      <td>40</td>
      <td>ARMY FLINTSTONES</td>
      <td>A Boring Saga of a Database Administrator And ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>148</td>
      <td>22.99</td>
      <td>R</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>40</th>
      <td>41</td>
      <td>ARSENIC INDEPENDENCE</td>
      <td>A Fanciful Documentary of a Mad Cow And a Woma...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>137</td>
      <td>17.99</td>
      <td>PG</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>41</th>
      <td>42</td>
      <td>ARTIST COLDBLOODED</td>
      <td>A Stunning Reflection of a Robot And a Moose w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>170</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>43</th>
      <td>44</td>
      <td>ATTACKS HATE</td>
      <td>A Fast-Paced Panorama of a Technical Writer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>113</td>
      <td>21.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>44</th>
      <td>45</td>
      <td>ATTRACTION NEWTON</td>
      <td>A Astounding Panorama of a Composer And a Fris...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>83</td>
      <td>14.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>46</th>
      <td>47</td>
      <td>BABY HALL</td>
      <td>A Boring Character Study of a A Shark And a Gi...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>153</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>50</th>
      <td>51</td>
      <td>BALLOON HOMEWARD</td>
      <td>A Insightful Panorama of a Forensic Psychologi...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>75</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>52</th>
      <td>53</td>
      <td>BANG KWAI</td>
      <td>A Epic Drama of a Madman And a Cat who must Fa...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>87</td>
      <td>25.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>53</th>
      <td>54</td>
      <td>BANGER PINOCCHIO</td>
      <td>A Awe-Inspiring Drama of a Car And a Pastry Ch...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>113</td>
      <td>15.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>56</th>
      <td>57</td>
      <td>BASIC EASY</td>
      <td>A Stunning Epistle of a Man And a Husband who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>90</td>
      <td>18.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>58</th>
      <td>59</td>
      <td>BEAR GRACELAND</td>
      <td>A Astounding Saga of a Dog And a Boy who must ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>160</td>
      <td>20.99</td>
      <td>R</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>60</th>
      <td>61</td>
      <td>BEAUTY GREASE</td>
      <td>A Fast-Paced Display of a Composer And a Moose...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>175</td>
      <td>28.99</td>
      <td>G</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>61</th>
      <td>62</td>
      <td>BED HIGHBALL</td>
      <td>A Astounding Panorama of a Lumberjack And a Do...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>106</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>66</th>
      <td>67</td>
      <td>BERETS AGENT</td>
      <td>A Taut Saga of a Crocodile And a Boy who must ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>77</td>
      <td>24.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>67</th>
      <td>68</td>
      <td>BETRAYED REAR</td>
      <td>A Emotional Character Study of a Boat And a Pi...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>122</td>
      <td>26.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>72</th>
      <td>73</td>
      <td>BINGO TALENTED</td>
      <td>A Touching Tale of a Girl And a Crocodile who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>150</td>
      <td>22.99</td>
      <td>PG-13</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>73</th>
      <td>74</td>
      <td>BIRCH ANTITRUST</td>
      <td>A Fanciful Panorama of a Husband And a Pioneer...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>162</td>
      <td>18.99</td>
      <td>PG</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>75</th>
      <td>76</td>
      <td>BIRDCAGE CASPER</td>
      <td>A Fast-Paced Saga of a Frisbee And a Astronaut...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>103</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>76</th>
      <td>77</td>
      <td>BIRDS PERDITION</td>
      <td>A Boring Story of a Womanizer And a Pioneer wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>61</td>
      <td>15.99</td>
      <td>G</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>906</th>
      <td>907</td>
      <td>TRANSLATION SUMMER</td>
      <td>A Touching Reflection of a Man And a Monkey wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>168</td>
      <td>10.99</td>
      <td>PG-13</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>917</th>
      <td>918</td>
      <td>TWISTED PIRATES</td>
      <td>A Touching Display of a Frisbee And a Boat who...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>152</td>
      <td>23.99</td>
      <td>PG</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>925</th>
      <td>926</td>
      <td>UNTOUCHABLES SUNRISE</td>
      <td>A Amazing Documentary of a Woman And a Astrona...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>120</td>
      <td>11.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>927</th>
      <td>928</td>
      <td>UPTOWN YOUNG</td>
      <td>A Fateful Documentary of a Dog And a Hunter wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>84</td>
      <td>16.99</td>
      <td>PG</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>928</th>
      <td>929</td>
      <td>USUAL UNTOUCHABLES</td>
      <td>A Touching Display of a Explorer And a Lumberj...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>128</td>
      <td>21.99</td>
      <td>PG-13</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>929</th>
      <td>930</td>
      <td>VACATION BOONDOCK</td>
      <td>A Fanciful Character Study of a Secret Agent A...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>145</td>
      <td>23.99</td>
      <td>R</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>932</th>
      <td>933</td>
      <td>VAMPIRE WHALE</td>
      <td>A Epic Story of a Lumberjack And a Monkey who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>126</td>
      <td>11.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>934</th>
      <td>935</td>
      <td>VANISHED GARDEN</td>
      <td>A Intrepid Character Study of a Squirrel And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>142</td>
      <td>17.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>938</th>
      <td>939</td>
      <td>VERTIGO NORTHWEST</td>
      <td>A Unbelieveable Display of a Mad Scientist And...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>90</td>
      <td>17.99</td>
      <td>R</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>940</th>
      <td>941</td>
      <td>VIDEOTAPE ARSENIC</td>
      <td>A Lacklusture Display of a Girl And a Astronau...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>145</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>942</th>
      <td>943</td>
      <td>VILLAIN DESPERATE</td>
      <td>A Boring Yarn of a Pioneer And a Feminist who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>76</td>
      <td>27.99</td>
      <td>PG-13</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>944</th>
      <td>945</td>
      <td>VIRGINIAN PLUTO</td>
      <td>A Emotional Panorama of a Dentist And a Crocod...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>164</td>
      <td>22.99</td>
      <td>R</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>946</th>
      <td>947</td>
      <td>VISION TORQUE</td>
      <td>A Thoughtful Documentary of a Dog And a Man wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>59</td>
      <td>16.99</td>
      <td>PG-13</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>959</th>
      <td>960</td>
      <td>WARS PLUTO</td>
      <td>A Taut Reflection of a Teacher And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>128</td>
      <td>15.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>962</th>
      <td>963</td>
      <td>WATCH TRACY</td>
      <td>A Fast-Paced Yarn of a Dog And a Frisbee who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>78</td>
      <td>12.99</td>
      <td>PG</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>963</th>
      <td>964</td>
      <td>WATERFRONT DELIVERANCE</td>
      <td>A Unbelieveable Documentary of a Dentist And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>61</td>
      <td>17.99</td>
      <td>G</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>966</th>
      <td>967</td>
      <td>WEEKEND PERSONAL</td>
      <td>A Fast-Paced Documentary of a Car And a Butler...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>134</td>
      <td>26.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>968</th>
      <td>969</td>
      <td>WEST LION</td>
      <td>A Intrepid Drama of a Butler And a Lumberjack ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>159</td>
      <td>29.99</td>
      <td>G</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>970</th>
      <td>971</td>
      <td>WHALE BIKINI</td>
      <td>A Intrepid Story of a Pastry Chef And a Databa...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>109</td>
      <td>11.99</td>
      <td>PG-13</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>971</th>
      <td>972</td>
      <td>WHISPERER GIANT</td>
      <td>A Intrepid Story of a Dentist And a Hunter who...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>59</td>
      <td>24.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>973</th>
      <td>974</td>
      <td>WILD APOLLO</td>
      <td>A Beautiful Story of a Monkey And a Sumo Wrest...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>181</td>
      <td>24.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>979</th>
      <td>980</td>
      <td>WIZARD COLDBLOODED</td>
      <td>A Lacklusture Display of a Robot And a Girl wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>75</td>
      <td>12.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>981</th>
      <td>982</td>
      <td>WOMEN DORADO</td>
      <td>A Insightful Documentary of a Waitress And a B...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>126</td>
      <td>23.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>984</th>
      <td>985</td>
      <td>WONDERLAND CHRISTMAS</td>
      <td>A Awe-Inspiring Character Study of a Waitress ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>111</td>
      <td>19.99</td>
      <td>PG</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>988</th>
      <td>989</td>
      <td>WORKING MICROCOSMOS</td>
      <td>A Stunning Epistle of a Dentist And a Dog who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>74</td>
      <td>22.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>990</th>
      <td>991</td>
      <td>WORST BANGER</td>
      <td>A Thrilling Drama of a Madman And a Dentist wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>185</td>
      <td>26.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>991</th>
      <td>992</td>
      <td>WRATH MILE</td>
      <td>A Intrepid Reflection of a Technical Writer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>176</td>
      <td>17.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>994</th>
      <td>995</td>
      <td>YENTL IDAHO</td>
      <td>A Amazing Display of a Robot And a Astronaut w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>86</td>
      <td>11.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>996</th>
      <td>997</td>
      <td>YOUTH KICK</td>
      <td>A Touching Drama of a Teacher And a Cat who mu...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>179</td>
      <td>14.99</td>
      <td>NC-17</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>998</th>
      <td>999</td>
      <td>ZOOLANDER FICTION</td>
      <td>A Fateful Reflection of a Waitress And a Boat ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>101</td>
      <td>28.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
  </tbody>
</table>
<p>394 rows × 13 columns</p>
</div>



# NULL


```python
# SELECT * 
# FROM sakila.film
# where original_language_id is null;

film_df[film_df.original_language_id.isnull()]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
      <th>release_year</th>
      <th>language_id</th>
      <th>original_language_id</th>
      <th>rental_duration</th>
      <th>rental_rate</th>
      <th>length</th>
      <th>replacement_cost</th>
      <th>rating</th>
      <th>special_features</th>
      <th>last_update</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ACADEMY DINOSAUR</td>
      <td>A Epic Drama of a Feminist And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>86</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>ACE GOLDFINGER</td>
      <td>A Astounding Epistle of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>48</td>
      <td>12.99</td>
      <td>G</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ADAPTATION HOLES</td>
      <td>A Astounding Reflection of a Lumberjack And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>AFFAIR PREJUDICE</td>
      <td>A Fanciful Documentary of a Frisbee And a Lumb...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>117</td>
      <td>26.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>AFRICAN EGG</td>
      <td>A Fast-Paced Documentary of a Pastry Chef And ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>130</td>
      <td>22.99</td>
      <td>G</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>AGENT TRUMAN</td>
      <td>A Intrepid Panorama of a Robot And a Boy who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>169</td>
      <td>17.99</td>
      <td>PG</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>AIRPLANE SIERRA</td>
      <td>A Touching Saga of a Hunter And a Butler who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>62</td>
      <td>28.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>AIRPORT POLLOCK</td>
      <td>A Epic Tale of a Moose And a Girl who must Con...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>54</td>
      <td>15.99</td>
      <td>R</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>ALABAMA DEVIL</td>
      <td>A Thoughtful Panorama of a Database Administra...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>114</td>
      <td>21.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>ALADDIN CALENDAR</td>
      <td>A Action-Packed Tale of a Man And a Lumberjack...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>63</td>
      <td>24.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>ALAMO VIDEOTAPE</td>
      <td>A Boring Epistle of a Butler And a Cat who mus...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>126</td>
      <td>16.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>ALASKA PHANTOM</td>
      <td>A Fanciful Saga of a Hunter And a Pastry Chef ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>136</td>
      <td>22.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>ALI FOREVER</td>
      <td>A Action-Packed Drama of a Dentist And a Croco...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>150</td>
      <td>21.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>ALICE FANTASIA</td>
      <td>A Emotional Drama of a A Shark And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>94</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15</td>
      <td>ALIEN CENTER</td>
      <td>A Brilliant Drama of a Cat And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>46</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>ALLEY EVOLUTION</td>
      <td>A Fast-Paced Drama of a Robot And a Composer w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>180</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>ALONE TRIP</td>
      <td>A Fast-Paced Character Study of a Composer And...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>82</td>
      <td>14.99</td>
      <td>R</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>ALTER VICTORY</td>
      <td>A Thoughtful Drama of a Composer And a Feminis...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>57</td>
      <td>27.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>AMADEUS HOLY</td>
      <td>A Emotional Display of a Pioneer And a Technic...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>113</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>AMELIE HELLFIGHTERS</td>
      <td>A Boring Drama of a Woman And a Squirrel who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>79</td>
      <td>23.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>AMERICAN CIRCUS</td>
      <td>A Insightful Drama of a Girl And a Astronaut w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>129</td>
      <td>17.99</td>
      <td>R</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>21</th>
      <td>22</td>
      <td>AMISTAD MIDSUMMER</td>
      <td>A Emotional Character Study of a Dentist And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>85</td>
      <td>10.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>22</th>
      <td>23</td>
      <td>ANACONDA CONFESSIONS</td>
      <td>A Lacklusture Display of a Dentist And a Denti...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>92</td>
      <td>9.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>23</th>
      <td>24</td>
      <td>ANALYZE HOOSIERS</td>
      <td>A Thoughtful Display of a Explorer And a Pastr...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>181</td>
      <td>19.99</td>
      <td>R</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>24</th>
      <td>25</td>
      <td>ANGELS LIFE</td>
      <td>A Thoughtful Display of a Woman And a Astronau...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>74</td>
      <td>15.99</td>
      <td>G</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>ANNIE IDENTITY</td>
      <td>A Amazing Panorama of a Pastry Chef And a Boat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>86</td>
      <td>15.99</td>
      <td>G</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>26</th>
      <td>27</td>
      <td>ANONYMOUS HUMAN</td>
      <td>A Amazing Reflection of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>0.99</td>
      <td>179</td>
      <td>12.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>27</th>
      <td>28</td>
      <td>ANTHEM LUKE</td>
      <td>A Touching Panorama of a Waitress And a Woman ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>91</td>
      <td>16.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29</td>
      <td>ANTITRUST TOMATOES</td>
      <td>A Fateful Yarn of a Womanizer And a Feminist w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>168</td>
      <td>11.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>29</th>
      <td>30</td>
      <td>ANYTHING SAVANNAH</td>
      <td>A Epic Story of a Pastry Chef And a Woman who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>82</td>
      <td>27.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>970</th>
      <td>971</td>
      <td>WHALE BIKINI</td>
      <td>A Intrepid Story of a Pastry Chef And a Databa...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>109</td>
      <td>11.99</td>
      <td>PG-13</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>971</th>
      <td>972</td>
      <td>WHISPERER GIANT</td>
      <td>A Intrepid Story of a Dentist And a Hunter who...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>59</td>
      <td>24.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>972</th>
      <td>973</td>
      <td>WIFE TURN</td>
      <td>A Awe-Inspiring Epistle of a Teacher And a Fem...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>183</td>
      <td>27.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>973</th>
      <td>974</td>
      <td>WILD APOLLO</td>
      <td>A Beautiful Story of a Monkey And a Sumo Wrest...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>181</td>
      <td>24.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>974</th>
      <td>975</td>
      <td>WILLOW TRACY</td>
      <td>A Brilliant Panorama of a Boat And a Astronaut...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>137</td>
      <td>22.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>975</th>
      <td>976</td>
      <td>WIND PHANTOM</td>
      <td>A Touching Saga of a Madman And a Forensic Psy...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>111</td>
      <td>12.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>976</th>
      <td>977</td>
      <td>WINDOW SIDE</td>
      <td>A Astounding Character Study of a Womanizer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>85</td>
      <td>25.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>977</th>
      <td>978</td>
      <td>WISDOM WORKER</td>
      <td>A Unbelieveable Saga of a Forensic Psychologis...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>98</td>
      <td>12.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>978</th>
      <td>979</td>
      <td>WITCHES PANIC</td>
      <td>A Awe-Inspiring Drama of a Secret Agent And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>100</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>979</th>
      <td>980</td>
      <td>WIZARD COLDBLOODED</td>
      <td>A Lacklusture Display of a Robot And a Girl wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>75</td>
      <td>12.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>980</th>
      <td>981</td>
      <td>WOLVES DESIRE</td>
      <td>A Fast-Paced Drama of a Squirrel And a Robot w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>0.99</td>
      <td>55</td>
      <td>13.99</td>
      <td>NC-17</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>981</th>
      <td>982</td>
      <td>WOMEN DORADO</td>
      <td>A Insightful Documentary of a Waitress And a B...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>126</td>
      <td>23.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>982</th>
      <td>983</td>
      <td>WON DARES</td>
      <td>A Unbelieveable Documentary of a Teacher And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>105</td>
      <td>18.99</td>
      <td>PG</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>983</th>
      <td>984</td>
      <td>WONDERFUL DROP</td>
      <td>A Boring Panorama of a Woman And a Madman who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>126</td>
      <td>20.99</td>
      <td>NC-17</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>984</th>
      <td>985</td>
      <td>WONDERLAND CHRISTMAS</td>
      <td>A Awe-Inspiring Character Study of a Waitress ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>111</td>
      <td>19.99</td>
      <td>PG</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>985</th>
      <td>986</td>
      <td>WONKA SEA</td>
      <td>A Brilliant Saga of a Boat And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>85</td>
      <td>24.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>986</th>
      <td>987</td>
      <td>WORDS HUNTER</td>
      <td>A Action-Packed Reflection of a Composer And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>2.99</td>
      <td>116</td>
      <td>13.99</td>
      <td>PG</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>987</th>
      <td>988</td>
      <td>WORKER TARZAN</td>
      <td>A Action-Packed Yarn of a Secret Agent And a T...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>7</td>
      <td>2.99</td>
      <td>139</td>
      <td>26.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>988</th>
      <td>989</td>
      <td>WORKING MICROCOSMOS</td>
      <td>A Stunning Epistle of a Dentist And a Dog who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>4.99</td>
      <td>74</td>
      <td>22.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>989</th>
      <td>990</td>
      <td>WORLD LEATHERNECKS</td>
      <td>A Unbelieveable Tale of a Pioneer And a Astron...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>0.99</td>
      <td>171</td>
      <td>13.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>990</th>
      <td>991</td>
      <td>WORST BANGER</td>
      <td>A Thrilling Drama of a Madman And a Dentist wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>2.99</td>
      <td>185</td>
      <td>26.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>991</th>
      <td>992</td>
      <td>WRATH MILE</td>
      <td>A Intrepid Reflection of a Technical Writer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>0.99</td>
      <td>176</td>
      <td>17.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>992</th>
      <td>993</td>
      <td>WRONG BEHAVIOR</td>
      <td>A Emotional Saga of a Crocodile And a Sumo Wre...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>2.99</td>
      <td>178</td>
      <td>10.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>993</th>
      <td>994</td>
      <td>WYOMING STORM</td>
      <td>A Awe-Inspiring Panorama of a Robot And a Boat...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>4.99</td>
      <td>100</td>
      <td>29.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>994</th>
      <td>995</td>
      <td>YENTL IDAHO</td>
      <td>A Amazing Display of a Robot And a Astronaut w...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>4.99</td>
      <td>86</td>
      <td>11.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>995</th>
      <td>996</td>
      <td>YOUNG LANGUAGE</td>
      <td>A Unbelieveable Yarn of a Boat And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>183</td>
      <td>9.99</td>
      <td>G</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>996</th>
      <td>997</td>
      <td>YOUTH KICK</td>
      <td>A Touching Drama of a Teacher And a Cat who mu...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>4</td>
      <td>0.99</td>
      <td>179</td>
      <td>14.99</td>
      <td>NC-17</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>997</th>
      <td>998</td>
      <td>ZHIVAGO CORE</td>
      <td>A Fateful Yarn of a Composer And a Man who mus...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>6</td>
      <td>0.99</td>
      <td>105</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>998</th>
      <td>999</td>
      <td>ZOOLANDER FICTION</td>
      <td>A Fateful Reflection of a Waitress And a Boat ...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>5</td>
      <td>2.99</td>
      <td>101</td>
      <td>28.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>999</th>
      <td>1000</td>
      <td>ZORRO ARK</td>
      <td>A Intrepid Panorama of a Mad Scientist And a B...</td>
      <td>2006</td>
      <td>1</td>
      <td>None</td>
      <td>3</td>
      <td>4.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 13 columns</p>
</div>



# fill NULL
补全NULL值的操作pandas比sql方便


```python
# SELECT (case when original_language_id is null then 999 else original_language_id end ) as original_language_id
#  FROM sakila.film
# where original_language_id is null;

film_df.original_language_id.fillna(999,inplace=True)
film_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>film_id</th>
      <th>title</th>
      <th>description</th>
      <th>release_year</th>
      <th>language_id</th>
      <th>original_language_id</th>
      <th>rental_duration</th>
      <th>rental_rate</th>
      <th>length</th>
      <th>replacement_cost</th>
      <th>rating</th>
      <th>special_features</th>
      <th>last_update</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ACADEMY DINOSAUR</td>
      <td>A Epic Drama of a Feminist And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>86</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>ACE GOLDFINGER</td>
      <td>A Astounding Epistle of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>4.99</td>
      <td>48</td>
      <td>12.99</td>
      <td>G</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ADAPTATION HOLES</td>
      <td>A Astounding Reflection of a Lumberjack And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>7</td>
      <td>2.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>AFFAIR PREJUDICE</td>
      <td>A Fanciful Documentary of a Frisbee And a Lumb...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>2.99</td>
      <td>117</td>
      <td>26.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>AFRICAN EGG</td>
      <td>A Fast-Paced Documentary of a Pastry Chef And ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>130</td>
      <td>22.99</td>
      <td>G</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>AGENT TRUMAN</td>
      <td>A Intrepid Panorama of a Robot And a Boy who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>2.99</td>
      <td>169</td>
      <td>17.99</td>
      <td>PG</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>AIRPLANE SIERRA</td>
      <td>A Touching Saga of a Hunter And a Butler who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>4.99</td>
      <td>62</td>
      <td>28.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>AIRPORT POLLOCK</td>
      <td>A Epic Tale of a Moose And a Girl who must Con...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>4.99</td>
      <td>54</td>
      <td>15.99</td>
      <td>R</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>ALABAMA DEVIL</td>
      <td>A Thoughtful Panorama of a Database Administra...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>2.99</td>
      <td>114</td>
      <td>21.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>ALADDIN CALENDAR</td>
      <td>A Action-Packed Tale of a Man And a Lumberjack...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>4.99</td>
      <td>63</td>
      <td>24.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>ALAMO VIDEOTAPE</td>
      <td>A Boring Epistle of a Butler And a Cat who mus...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>126</td>
      <td>16.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>ALASKA PHANTOM</td>
      <td>A Fanciful Saga of a Hunter And a Pastry Chef ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>136</td>
      <td>22.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>ALI FOREVER</td>
      <td>A Action-Packed Drama of a Dentist And a Croco...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>150</td>
      <td>21.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>ALICE FANTASIA</td>
      <td>A Emotional Drama of a A Shark And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>94</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15</td>
      <td>ALIEN CENTER</td>
      <td>A Brilliant Drama of a Cat And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>2.99</td>
      <td>46</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>ALLEY EVOLUTION</td>
      <td>A Fast-Paced Drama of a Robot And a Composer w...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>180</td>
      <td>23.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>ALONE TRIP</td>
      <td>A Fast-Paced Character Study of a Composer And...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>0.99</td>
      <td>82</td>
      <td>14.99</td>
      <td>R</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>ALTER VICTORY</td>
      <td>A Thoughtful Drama of a Composer And a Feminis...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>57</td>
      <td>27.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>AMADEUS HOLY</td>
      <td>A Emotional Display of a Pioneer And a Technic...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>113</td>
      <td>20.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>AMELIE HELLFIGHTERS</td>
      <td>A Boring Drama of a Woman And a Squirrel who m...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>79</td>
      <td>23.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>AMERICAN CIRCUS</td>
      <td>A Insightful Drama of a Girl And a Astronaut w...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>4.99</td>
      <td>129</td>
      <td>17.99</td>
      <td>R</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>21</th>
      <td>22</td>
      <td>AMISTAD MIDSUMMER</td>
      <td>A Emotional Character Study of a Dentist And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>85</td>
      <td>10.99</td>
      <td>G</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>22</th>
      <td>23</td>
      <td>ANACONDA CONFESSIONS</td>
      <td>A Lacklusture Display of a Dentist And a Denti...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>0.99</td>
      <td>92</td>
      <td>9.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>23</th>
      <td>24</td>
      <td>ANALYZE HOOSIERS</td>
      <td>A Thoughtful Display of a Explorer And a Pastr...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>181</td>
      <td>19.99</td>
      <td>R</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>24</th>
      <td>25</td>
      <td>ANGELS LIFE</td>
      <td>A Thoughtful Display of a Woman And a Astronau...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>2.99</td>
      <td>74</td>
      <td>15.99</td>
      <td>G</td>
      <td>Trailers</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>ANNIE IDENTITY</td>
      <td>A Amazing Panorama of a Pastry Chef And a Boat...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>0.99</td>
      <td>86</td>
      <td>15.99</td>
      <td>G</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>26</th>
      <td>27</td>
      <td>ANONYMOUS HUMAN</td>
      <td>A Amazing Reflection of a Database Administrat...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>7</td>
      <td>0.99</td>
      <td>179</td>
      <td>12.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>27</th>
      <td>28</td>
      <td>ANTHEM LUKE</td>
      <td>A Touching Panorama of a Waitress And a Woman ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>4.99</td>
      <td>91</td>
      <td>16.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29</td>
      <td>ANTITRUST TOMATOES</td>
      <td>A Fateful Yarn of a Womanizer And a Feminist w...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>2.99</td>
      <td>168</td>
      <td>11.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>29</th>
      <td>30</td>
      <td>ANYTHING SAVANNAH</td>
      <td>A Epic Story of a Pastry Chef And a Woman who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>2.99</td>
      <td>82</td>
      <td>27.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>970</th>
      <td>971</td>
      <td>WHALE BIKINI</td>
      <td>A Intrepid Story of a Pastry Chef And a Databa...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>109</td>
      <td>11.99</td>
      <td>PG-13</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>971</th>
      <td>972</td>
      <td>WHISPERER GIANT</td>
      <td>A Intrepid Story of a Dentist And a Hunter who...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>59</td>
      <td>24.99</td>
      <td>PG-13</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>972</th>
      <td>973</td>
      <td>WIFE TURN</td>
      <td>A Awe-Inspiring Epistle of a Teacher And a Fem...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>4.99</td>
      <td>183</td>
      <td>27.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>973</th>
      <td>974</td>
      <td>WILD APOLLO</td>
      <td>A Beautiful Story of a Monkey And a Sumo Wrest...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>0.99</td>
      <td>181</td>
      <td>24.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes,Behind th...</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>974</th>
      <td>975</td>
      <td>WILLOW TRACY</td>
      <td>A Brilliant Panorama of a Boat And a Astronaut...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>137</td>
      <td>22.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>975</th>
      <td>976</td>
      <td>WIND PHANTOM</td>
      <td>A Touching Saga of a Madman And a Forensic Psy...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>111</td>
      <td>12.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>976</th>
      <td>977</td>
      <td>WINDOW SIDE</td>
      <td>A Astounding Character Study of a Womanizer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>2.99</td>
      <td>85</td>
      <td>25.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>977</th>
      <td>978</td>
      <td>WISDOM WORKER</td>
      <td>A Unbelieveable Saga of a Forensic Psychologis...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>0.99</td>
      <td>98</td>
      <td>12.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>978</th>
      <td>979</td>
      <td>WITCHES PANIC</td>
      <td>A Awe-Inspiring Drama of a Secret Agent And a ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>4.99</td>
      <td>100</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>979</th>
      <td>980</td>
      <td>WIZARD COLDBLOODED</td>
      <td>A Lacklusture Display of a Robot And a Girl wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>75</td>
      <td>12.99</td>
      <td>PG</td>
      <td>Commentaries,Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>980</th>
      <td>981</td>
      <td>WOLVES DESIRE</td>
      <td>A Fast-Paced Drama of a Squirrel And a Robot w...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>7</td>
      <td>0.99</td>
      <td>55</td>
      <td>13.99</td>
      <td>NC-17</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>981</th>
      <td>982</td>
      <td>WOMEN DORADO</td>
      <td>A Insightful Documentary of a Waitress And a B...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>0.99</td>
      <td>126</td>
      <td>23.99</td>
      <td>R</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>982</th>
      <td>983</td>
      <td>WON DARES</td>
      <td>A Unbelieveable Documentary of a Teacher And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>7</td>
      <td>2.99</td>
      <td>105</td>
      <td>18.99</td>
      <td>PG</td>
      <td>Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>983</th>
      <td>984</td>
      <td>WONDERFUL DROP</td>
      <td>A Boring Panorama of a Woman And a Madman who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>2.99</td>
      <td>126</td>
      <td>20.99</td>
      <td>NC-17</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>984</th>
      <td>985</td>
      <td>WONDERLAND CHRISTMAS</td>
      <td>A Awe-Inspiring Character Study of a Waitress ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>111</td>
      <td>19.99</td>
      <td>PG</td>
      <td>Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>985</th>
      <td>986</td>
      <td>WONKA SEA</td>
      <td>A Brilliant Saga of a Boat And a Mad Scientist...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>85</td>
      <td>24.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>986</th>
      <td>987</td>
      <td>WORDS HUNTER</td>
      <td>A Action-Packed Reflection of a Composer And a...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>2.99</td>
      <td>116</td>
      <td>13.99</td>
      <td>PG</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>987</th>
      <td>988</td>
      <td>WORKER TARZAN</td>
      <td>A Action-Packed Yarn of a Secret Agent And a T...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>7</td>
      <td>2.99</td>
      <td>139</td>
      <td>26.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>988</th>
      <td>989</td>
      <td>WORKING MICROCOSMOS</td>
      <td>A Stunning Epistle of a Dentist And a Dog who ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>4.99</td>
      <td>74</td>
      <td>22.99</td>
      <td>R</td>
      <td>Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>989</th>
      <td>990</td>
      <td>WORLD LEATHERNECKS</td>
      <td>A Unbelieveable Tale of a Pioneer And a Astron...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>0.99</td>
      <td>171</td>
      <td>13.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>990</th>
      <td>991</td>
      <td>WORST BANGER</td>
      <td>A Thrilling Drama of a Madman And a Dentist wh...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>2.99</td>
      <td>185</td>
      <td>26.99</td>
      <td>PG</td>
      <td>Deleted Scenes,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>991</th>
      <td>992</td>
      <td>WRATH MILE</td>
      <td>A Intrepid Reflection of a Technical Writer An...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>0.99</td>
      <td>176</td>
      <td>17.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>992</th>
      <td>993</td>
      <td>WRONG BEHAVIOR</td>
      <td>A Emotional Saga of a Crocodile And a Sumo Wre...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>2.99</td>
      <td>178</td>
      <td>10.99</td>
      <td>PG-13</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>993</th>
      <td>994</td>
      <td>WYOMING STORM</td>
      <td>A Awe-Inspiring Panorama of a Robot And a Boat...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>4.99</td>
      <td>100</td>
      <td>29.99</td>
      <td>PG-13</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>994</th>
      <td>995</td>
      <td>YENTL IDAHO</td>
      <td>A Amazing Display of a Robot And a Astronaut w...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>4.99</td>
      <td>86</td>
      <td>11.99</td>
      <td>R</td>
      <td>Trailers,Commentaries,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>995</th>
      <td>996</td>
      <td>YOUNG LANGUAGE</td>
      <td>A Unbelieveable Yarn of a Boat And a Database ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>183</td>
      <td>9.99</td>
      <td>G</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>996</th>
      <td>997</td>
      <td>YOUTH KICK</td>
      <td>A Touching Drama of a Teacher And a Cat who mu...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>4</td>
      <td>0.99</td>
      <td>179</td>
      <td>14.99</td>
      <td>NC-17</td>
      <td>Trailers,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>997</th>
      <td>998</td>
      <td>ZHIVAGO CORE</td>
      <td>A Fateful Yarn of a Composer And a Man who mus...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>6</td>
      <td>0.99</td>
      <td>105</td>
      <td>10.99</td>
      <td>NC-17</td>
      <td>Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>998</th>
      <td>999</td>
      <td>ZOOLANDER FICTION</td>
      <td>A Fateful Reflection of a Waitress And a Boat ...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>5</td>
      <td>2.99</td>
      <td>101</td>
      <td>28.99</td>
      <td>R</td>
      <td>Trailers,Deleted Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
    <tr>
      <th>999</th>
      <td>1000</td>
      <td>ZORRO ARK</td>
      <td>A Intrepid Panorama of a Mad Scientist And a B...</td>
      <td>2006</td>
      <td>1</td>
      <td>999</td>
      <td>3</td>
      <td>4.99</td>
      <td>50</td>
      <td>18.99</td>
      <td>NC-17</td>
      <td>Trailers,Commentaries,Behind the Scenes</td>
      <td>2006-02-15 05:03:42</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 13 columns</p>
</div>



# order by


```python
# 按照评分从低到高对电影进行排序
# select rental_rate, film_id, title, description
# from film
# order by rental_rate
 
film_df.sort_values(['rental_rate'])[["rental_rate", "film_id", "title", "description"]]  # 默认升序 非原地
```


```python
# 按照评分从高到低对电影进行排序
# select rental_rate, film_id, title, description
# from film
# order by rental_rate desc
 
film_df.sort_values(['rental_rate'], ascending=0)[["rental_rate", "film_id", "title", "description"]]  # 降序 非原地
```

# group by


```python
# select rating, count(*)
# from film
# group by rating

film_df.rating.value_counts()
```




    PG-13    223
    NC-17    210
    R        195
    PG       194
    G        178
    Name: rating, dtype: int64




```python
# select release_year, rating, count(*)
# from film
# group by release_year, rating

film_df.groupby(["release_year", "rating"]).size()  # the result type is series, we can trans it to Dataframe by reset_index
```




    release_year  rating
    2006          G         178
                  NC-17     210
                  PG        194
                  PG-13     223
                  R         195
    dtype: int64




```python
# select release_year, rating, count(*) as counts
# from film
# group by release_year, rating

film_df.groupby(["release_year", "rating"]).size().reset_index(name="counts")  # the result type is Dataframe and rename the column
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>release_year</th>
      <th>rating</th>
      <th>counts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2006</td>
      <td>G</td>
      <td>178</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2006</td>
      <td>NC-17</td>
      <td>210</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2006</td>
      <td>PG</td>
      <td>194</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2006</td>
      <td>PG-13</td>
      <td>223</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2006</td>
      <td>R</td>
      <td>195</td>
    </tr>
  </tbody>
</table>
</div>




```python
# select  rating , count(distinct rental_duration)  as rental_duration_type_count
# from  film
# group by rating;

film_df.groupby('rating').rental_duration.nunique().reset_index(name="rental_duration_type_count")
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>rating</th>
      <th>rental_duration_type_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>G</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NC-17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PG</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PG-13</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>R</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# select rating, count(distinct length) as length_distinct_count, avg(length) as length_mean, avg(rental_rate) as rental_rate_mean
# from film
# group by rating;
```

## [DEPRECATED] Dictionary groupby format
使用一种叫Dictionary groupby format的方式，然后droplevel(0)，最后reset_index


```python
new_df = film_df.groupby("rating").agg({"length": {"length_distinct_count": lambda x: x.nunique(), 
                                                   "length_mean": "mean"},
                                        "rental_rate": {"rental_rate_mean": "mean"}})
new_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">length</th>
      <th>rental_rate</th>
    </tr>
    <tr>
      <th></th>
      <th>length_distinct_count</th>
      <th>length_mean</th>
      <th>rental_rate_mean</th>
    </tr>
    <tr>
      <th>rating</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>G</th>
      <td>109</td>
      <td>111.050562</td>
      <td>2.888876</td>
    </tr>
    <tr>
      <th>NC-17</th>
      <td>110</td>
      <td>113.228571</td>
      <td>2.970952</td>
    </tr>
    <tr>
      <th>PG</th>
      <td>106</td>
      <td>112.005155</td>
      <td>3.051856</td>
    </tr>
    <tr>
      <th>PG-13</th>
      <td>116</td>
      <td>120.443946</td>
      <td>3.034843</td>
    </tr>
    <tr>
      <th>R</th>
      <td>103</td>
      <td>118.661538</td>
      <td>2.938718</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df.columns = new_df.columns.droplevel(0)
new_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>length_distinct_count</th>
      <th>length_mean</th>
      <th>rental_rate_mean</th>
    </tr>
    <tr>
      <th>rating</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>G</th>
      <td>109</td>
      <td>111.050562</td>
      <td>2.888876</td>
    </tr>
    <tr>
      <th>NC-17</th>
      <td>110</td>
      <td>113.228571</td>
      <td>2.970952</td>
    </tr>
    <tr>
      <th>PG</th>
      <td>106</td>
      <td>112.005155</td>
      <td>3.051856</td>
    </tr>
    <tr>
      <th>PG-13</th>
      <td>116</td>
      <td>120.443946</td>
      <td>3.034843</td>
    </tr>
    <tr>
      <th>R</th>
      <td>103</td>
      <td>118.661538</td>
      <td>2.938718</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df.reset_index()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>rating</th>
      <th>length_distinct_count</th>
      <th>length_mean</th>
      <th>rental_rate_mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>G</td>
      <td>109</td>
      <td>111.050562</td>
      <td>2.888876</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NC-17</td>
      <td>110</td>
      <td>113.228571</td>
      <td>2.970952</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PG</td>
      <td>106</td>
      <td>112.005155</td>
      <td>3.051856</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PG-13</td>
      <td>116</td>
      <td>120.443946</td>
      <td>3.034843</td>
    </tr>
    <tr>
      <th>4</th>
      <td>R</td>
      <td>103</td>
      <td>118.661538</td>
      <td>2.938718</td>
    </tr>
  </tbody>
</table>
</div>



## Use 'named' functions instead of lambda's:
但是上面用嵌套字典的来重命名计算的字段的方式已经[DEPRECATED]了 (>=0.20.1),推荐的方式是用函数代替匿名函数


```python
def length_distinct_count(group):
    return group.nunique()
    
def length_mean(group):
    return group.mean()
    
def rental_rate_mean(group):
    return group.mean()
    
new_df = film_df.groupby("rating").agg({"length": [length_distinct_count, length_mean],
                                        "rental_rate": rental_rate_mean})
new_df

# 后面的做法同上
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">length</th>
      <th>rental_rate</th>
    </tr>
    <tr>
      <th></th>
      <th>length_distinct_count</th>
      <th>length_mean</th>
      <th>rental_rate_mean</th>
    </tr>
    <tr>
      <th>rating</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>G</th>
      <td>109</td>
      <td>111.050562</td>
      <td>2.888876</td>
    </tr>
    <tr>
      <th>NC-17</th>
      <td>110</td>
      <td>113.228571</td>
      <td>2.970952</td>
    </tr>
    <tr>
      <th>PG</th>
      <td>106</td>
      <td>112.005155</td>
      <td>3.051856</td>
    </tr>
    <tr>
      <th>PG-13</th>
      <td>116</td>
      <td>120.443946</td>
      <td>3.034843</td>
    </tr>
    <tr>
      <th>R</th>
      <td>103</td>
      <td>118.661538</td>
      <td>2.938718</td>
    </tr>
  </tbody>
</table>
</div>



# group_concat
*注：oralce里对应的函数是wm_concat*


```python
# SELECT actor_id, group_concat(film_id order by film_id separator ',')  as film_ids
# FROM sakila.film_actor
# group by actor_id;

film_actor_df['film_id_str'] = film_actor_df['film_id'].map(str)
film_actor_df.groupby('actor_id')['film_id_str'].apply(lambda x: ','.join(x)).reset_index(name="film_ids").head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>actor_id</th>
      <th>film_ids</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1,23,25,106,140,166,277,361,438,499,506,509,60...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3,31,47,105,132,145,226,249,314,321,357,369,39...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>17,40,42,87,111,185,289,329,336,341,393,441,45...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>23,25,56,62,79,87,355,379,398,463,490,616,635,...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>19,54,85,146,171,172,202,203,286,288,316,340,3...</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
