# Netflix Movies and TV Shows Data Analysis using SQL

![](https://github.com/najirh/netflix_sql_project/blob/main/logo.png)

## Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives

- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.


## Schema

```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```

## Business Problems and Solutions

SELECT * FROM netflix.netflix_titles;
use netflix;

-- Netflix Data Analysis using SQL
-- Solutions of 15 business problems

-- 1. Count the number of Movies vs TV Shows

SELECT 
	type,
	COUNT(*)
FROM netflix
GROUP BY 1;

-- 2. Find the most common rating for movies and TV shows

SELECT 
    type,
    rating AS most_frequent_rating
FROM netflix
GROUP BY type, rating
ORDER BY COUNT(*) DESC
LIMIT 1;

-- 3. List all movies released in a specific year (e.g., 2020)

SELECT * 
FROM netflix
WHERE release_year = 2020;

-- 4. Find the top 5 countries with the most content on Netflix

SELECT 
    country,
    COUNT(*) AS total_content
FROM netflix
WHERE country IS NOT NULL
GROUP BY country
ORDER BY total_content DESC
LIMIT 5;

-- 5. Identify the longest movie

SELECT 
    *
FROM netflix
WHERE type = 'Movie'
ORDER BY CAST(SUBSTRING_INDEX(duration, ' ', 1) AS UNSIGNED) DESC
LIMIT 1;

-- 6. Find content added in the last 5 years

SELECT *
FROM netflix
WHERE STR_TO_DATE(date_added, '%M %d, %Y') >= DATE_SUB(CURDATE(), INTERVAL 5 YEAR);

-- 7. Find all the movies/TV shows by director 'Haile Gerima'!

SELECT *
FROM netflix
WHERE director = "Haile Gerima";

-- 8. List all TV shows with more than 5 seasons

SELECT *
FROM netflix
WHERE type = 'TV Show'
  AND CAST(SUBSTRING_INDEX(duration, ' ', 1) AS UNSIGNED) > 5;

-- 9. Count the number of content items in each genre

SELECT 
    listed_in AS genre,
    COUNT(*) AS total_content
FROM netflix
WHERE listed_in IS NOT NULL
GROUP BY listed_in;


-- 10. Find each year and the average numbers of content release by India on netflix. 
-- return top 5 year with highest avg content release !

SELECT release_year, COUNT(*) AS total_release
FROM netflix
WHERE country = 'India'
GROUP BY release_year
ORDER BY total_release DESC
LIMIT 5;

-- 11. List all movies that are documentaries

SELECT * FROM netflix
WHERE listed_in LIKE '%Documentaries';

-- 12. Find all content without a director

SELECT * 
FROM netflix
WHERE director IS NULL OR director = '';

-- 13. Find how many movies actor 'Junko Takeuchi' appeared in last 15 years!

SELECT *
FROM netflix
WHERE cast LIKE 'Junko Takeuchi%'
  AND release_year > YEAR(CURDATE()) - 15;

-- 14. Find the number of movies in each genre

SELECT 
    listed_in,
    COUNT(*) AS total_movies
FROM netflix
WHERE type = 'Movie'
GROUP BY listed_in;

-- 15. List all content (Movies/TV Shows) with rating TV-MA

SELECT *
FROM netflix
WHERE rating = 'TV-MA';
