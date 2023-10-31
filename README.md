# Apple Store (SQL Analysis project)

## 1. General Project Info
#### Project Overview
This data analysis is aim to provide insights into the apple store marketplace´s apps. The stakeholder is a fresh app developer who is interested about some data analysis before start to develop his own app. Information regarding most profitable genres, minimum features, and in general some recomendations that could hep him with decision making.

#### Data Source 
For this project we are going to use the following Apple Store dataset from [Kaggle.com](https://www.kaggle.com). It contains [several documents](https://github.com/Albertokam/SQL_applestore/tree/Images), so we would need to work on them in order to extract some information.

#### Tools
- [SQL Online Server](https://sqliteonline.com/)


## 2. Data Preparation
We have 5 files: 1 for Apple Store main characteristics (like name, rating, genre) and then 4 files of description field of each app. 
First, we will merge this 4 files into a new one called "applestore_description_combined", then we will work with only two files ("AppleStore" and "applestore_description_combined").

```sql
Create TABLE applestore_description_combined AS

Select * from appleStore_description1

Union ALL

Select * from appleStore_description2

Union ALL

Select * from appleStore_description3

Union ALL

SELECT * from appleStore_description4
```
![union_all](../Screenshots/1.png)
## 3. Exploratory Data Analysis (EDA)
3.1. We want to check if the info contained in both files is consistent. For this, we are going to check if the total number of apps included in both files is the same.
```sql
SELECT COUNT(DISTINCT id) as uniqueapp_ids
from AppleStore

SELECT COUNT(DISTINCT id) as uniqueapp_id2s
from applestore_description_combined
````
![distinct](../Screenshots/2.png)

3.2. Check for any missing key fileds value in both files
```sql
SELECT count(*) as missing_values
from AppleStore
where track_name is NULL OR user_rating is NULL or prime_genre is NULL

SELECT count(*) as missing_values2
from applestore_description_combined
where app_desc is NULL
```

![missing](../Screenshots/3.png)

The Dataset is quite well maintained as there are not missing values in key fields.

3.3. Find out the number of apps per genre 
```sql
SELECT prime_genre, count(*) as num_apps
from AppleStore 
group by prime_genre
order by num_apps DESC
```

![Top_Ten_genres](../Screenshots/4.png)

There is a huge difference between Games (which is the genre containing more apps) and the rest of genres. In fact, this is a good insight for the Stakeholder, since Games looks like a very competitive genre.

3.4. Get an overview of the apps ratings
```sql
Select  min(user_rating) as min_rating,
		max(user_rating) as max_rating,
        avg(user_rating) as avg_rating
FROM AppleStore
```

![rating_values](../Screenshots/5.png)

With this check we ensure the rating values make sense (they are between 0 and 5). This is a good sign again. In general, the Dataset seems to be quite comprehensive.







