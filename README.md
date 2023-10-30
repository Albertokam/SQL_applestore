# Apple Store (SQL Analysis project)

## 1. General Info
#### Project Overview
This data analysis is aim to provide insights into the apple store marketplace´s apps. The stakeholder is a fresh app developer who is interested about some data analysis before start to develop his own app. Information regarding most profitable genres, minimum features, and in general some recomendations that could hep him with decision making.

#### Data Source 
For this project we are going to use the following Apple Store dataset from [Kaggle.com](https://www.kaggle.com/datasets/leandrojnr/applestore-database). It contains several documents, so we would need to work on them in order to extract some information.

#### Tools
- [SQL Online Server](https://sqliteonline.com/)


## 2. Data Preparation
We have 5 files: 1 for Apple Store main characteristics (like name, rating, genre) and then 4 files of description field of each app. 
First, we will merge this 4 files into a new one called "applestore_description_combined", then we will work with only two files (AppleStore and applestore_description_combined).

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
![image](https://github.com/Albertokam/SQL_applestore/assets/149379816/caedc6f7-89ba-4bcd-ba04-57ca4cdd8ecd)

## 3. Exploratory Data Analysis (EDA)
3.1. We want to check if the info contained in both files is consistent. For this, we are going to check if the total number of apps included in both files is the same.
```sql
SELECT COUNT(DISTINCT id) as uniqueapp_ids
from AppleStore

SELECT COUNT(DISTINCT id) as uniqueapp_id2s
from applestore_description_combined
````
![image](https://github.com/Albertokam/SQL_applestore/assets/149379816/025b34a5-9821-4c81-9e86-bdeb6c60413a)
![image](https://github.com/Albertokam/SQL_applestore/assets/149379816/c8107b40-bed9-4e89-952b-4ccdb1579094)

3.2. Check for any missing key fileds value in both files
```sql
SELECT count(*) as missing_values
from AppleStore
where track_name is NULL OR user_rating is NULL or prime_genre is NULL

SELECT count(*) as missing_values2
from applestore_description_combined
where app_desc is NULL
```

![image](https://github.com/Albertokam/SQL_applestore/assets/149379816/808bc5c9-ba66-41d9-a39b-026de16b2ddc)
![image](https://github.com/Albertokam/SQL_applestore/assets/149379816/0b237ede-9edc-4ca5-bb1d-a6efa8eff1aa)

The Dataset is quite well maintained as there are not missing values in key fields.

3.3. Find out the number of apps per genre 
´´´sql
SELECT prime_genre, count(*) as num_apps
from AppleStore 
group by prime_genre
order by num_apps DESC
```


