

```python
import pandas as pd
import pymysql
```


```python
conn = pymysql.connect(host="127.0.0.1", user="root", passwd="root", db="sakila", port=3306, charset='utf8')
film_df = pd.read_sql("select * from film;", conn)
film_actor_df = pd.read_sql("select * from film_actor;", conn)
actor_df = pd.read_sql("select * from actor;", conn)

```

# desc


```python
# desc film;
# film_id	smallint(5) unsigned	NO	PRI		auto_increment
# title	varchar(255)	NO	MUL		
# description	text	YES			
# release_year	year(4)	YES			
# language_id	tinyint(3) unsigned	NO	MUL		
# original_language_id	tinyint(3) unsigned	YES	MUL		
# rental_duration	tinyint(3) unsigned	NO		3	
# rental_rate	decimal(4,2)	NO		4.99	
# length	smallint(5) unsigned	YES			
# replacement_cost	decimal(5,2)	NO		19.99	
# rating	enum('G','PG','PG-13','R','NC-17')	YES		G	
# special_features	set('Trailers','Commentaries','Deleted Scenes','Behind the Scenes')	YES			
# last_update	timestamp	NO		CURRENT_TIMESTAMP	on update CURRENT_TIMESTAMP
```


```python
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


