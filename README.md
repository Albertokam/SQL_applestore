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

## 4. Data Analysis
4.1. Check if paid apps have higher rating

```sql
SELECT CASE
	WHEN price > 0 THEN 'PAID'
	ELSE 'FREE'
	END AS app_type,
	avg(user_rating) as avg_rating
FROM AppleStore
	GROUP BY app_type
```
![paid_unpaid](../Screenshots/6.png)

We can see, that paid apps have a slightly higher rating in average.

4.2. Check if apps with more supported languages have higher rating

```sql
SELECT CASE
	WHEN lang_num < 10 then '<10 languages'
    	WHen lang_num BETWEEN 10 AND 30 then '10<30 languages'
    	When lang_num > 30 then '>30 languages'
    	END as language_bucket,
    	avg(user_rating) as avg_rating
FROM AppleStore
            GROUP by language_bucket
            ORDER BY avg_rating DESC
```
![languages](../Screenshots/7.png)

Here, we can observe that more number of languages is not linked to a higher rating. So, the app should concentrate on the target languages rather than try to include more than needed.

4.3. Check genres with low rating in order to see opportunities for market penetration 

```sql
	SELECT 	prime_genre, 
		avg(user_rating) as avg_rating
       	FROM AppleStore
        	GROUP BY prime_genre
        	order by avg_rating ASC
        	LIMIT 10
```
![lowest](../Screenshots/8.png)

This could be interensting in case stakeholders wanted to penetrate a low performance genre with a really good app and get all the attention from the users.

4.4. Check if there is any correlation between app description lenght and rating

```sql
	SELECT CASE
	When length (b.app_desc)< 100 then 'short'
        When length (b.app_desc) BETWEEN 100 and 1000 then 'medium'
        ELSE 'long'
        END as description_length_bucket,
        avg(user_rating) as avg_rating
        
FROM AppleStore as a 
JOIN applestore_description_combined as b 
ON a.id = b.id

	group by description_length_bucket
        order by avg_rating DESC
```
![description_lenght](../Screenshots/9.png)

This is actually quite a finding, since the lenght of the description is directly correlate to the rating. So stakeholders should spend time on this topic.

4.5. Check top-rated apps for each genre 
```sql
SELECT 
		prime_genre,
        track_name,
        user_rating
FROM (
		SELECT 
		prime_genre,
        track_name,
        user_rating,
  		RANK() OVER(PARTITION BY prime_genre order BY user_rating DESC, rating_count_tot DESC) AS rank
  		FROM
  		AppleStore) as a 
WHERE
a.rank=1
```
![top_rated](../Screenshots/10.png)

This is just a summary of top-rated apps that could be used in case we want to check them directly from the applestore and see how they look like, design aspects, options, etc...
