# Netflix Movies TV Shows and Data Analysis using SQL

![](https://github.com/MANISH-KUMAR1902/netflix/blob/main/download.png)

## Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives

- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:** [Movies Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)

## schemas
```sql
  --drop the table if having name netflix_titles
  drop table if exists netflix_titles;
  --create the table
  create table netflix_titles(
  	show_id varchar(10) primary key,
  	type varchar(15),
  	title varchar(1000),
  	director varchar(500),
  	casts varchar(1500),
  	country varchar(500),
  	date_added varchar(100),
  	release_year int,
  	rating varchar(25),
  	duration varchar(20),
  	listed_in varchar(2000),
  	description varchar(400)
  );
  ```
### 1. Count the Number of Movies vs TV Shows

```sql
SELECT 
    type,
    COUNT(*)
FROM netflix
GROUP BY show_id;
```

## 2. find duplicate duplicate remove 
```sql
  select * from netflix_titles
  where title in(
  select title
  from netflix_titles
  group by title
  having count(*) >1)
  order by title
  
  with cte as(
  	select 
  			*,row_number() over(partition by title order by show_id) rnk
  	from netflix_titles
  )
  select * from cte 
  where rnk>1
  order by show_id
```

## 3. remove the duplicate rows 
```sql
  WITH CTE AS (
      SELECT *,
             ROW_NUMBER() OVER (PARTITION BY title ORDER BY show_id) AS rn
      FROM netflix_titles
  )
  DELETE FROM netflix_titles
  WHERE show_id IN (
      SELECT show_id FROM CTE WHERE rn > 1
  );
```
