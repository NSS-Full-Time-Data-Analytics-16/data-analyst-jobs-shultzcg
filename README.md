The dataset for this exercise has been derived from the `Indeed Data Scientist/Analyst/Engineer` [dataset](https://www.kaggle.com/elroyggj/indeed-dataset-data-scientistanalystengineer) on kaggle.com. 

Before beginning to answer questions, take some time to review the data dictionary and familiarize yourself with the data that is contained in each column.

#### Provide the SQL queries and answers for the following questions/tasks using the data_analyst_jobs table you have created in PostgreSQL:

1.	How many rows are in the data_analyst_jobs table?
SELECT *
FROM data_analyst_jobs;

Total Rows: 1793

2.	Write a query to look at just the first 10 rows. What company is associated with the job posting on the 10th row?
SELECT company
FROM data_analyst_jobs
LIMIT 10;

10TH Row = XTO Land Data Analyst

3.	How many postings are in Tennessee? How many are there in either Tennessee or Kentucky?
SELECT COUNT (location)
FROM data_analyst_jobs
WHERE location = 'TN';

Postings in TN = 21

SELECT COUNT (location)
FROM data_analyst_jobs
WHERE location = 'TN' OR location = 'KY';

Postings in TN OR KY = 27

4.	How many postings in Tennessee have a star rating above 4?
SELECT COUNT (location)
FROM data_analyst_jobs
WHERE location = 'TN' AND star_rating > 4;

TN Postings w/ > 4 Star Rating = 3

5.	How many postings in the dataset have a review count between 500 and 1000?
SELECT COUNT (review_count)
FROM data_analyst_jobs
WHERE review_count >= 500 AND review_count <=1000;

Postings w/ 500-1000 review counts = 151

6.	Show the average star rating for companies in each state. The output should show the state as `state` and the average rating for the state as `avg_rating`. Which state shows the highest average rating?
SELECT location AS state, ROUND (AVG (star_rating), 1) AS avg_rating
FROM data_analyst_jobs
GROUP BY state
ORDER BY avg_rating DESC;

State w/ Highest Average Rating = NE (Nebraska) @ 4.2

7.	Select unique job titles from the data_analyst_jobs table. How many are there?
SELECT COUNT (DISTINCT title)
FROM data_analyst_jobs;

Number of Unique Job Titles = 881

8.	How many unique job titles are there for California companies?
SELECT COUNT (DISTINCT title)
FROM data_analyst_jobs
WHERE location = 'CA'

Number of Unique Job Titles in CA = 230

9.	Find the name of each company and its average star rating for all companies that have more than 5000 reviews across all locations. How many companies are there with more that 5000 reviews across all locations?
SELECT company, ROUND (AVG (star_rating), 1) AS avg_star_rating
FROM data_analyst_jobs
WHERE review_count > 5000 AND company IS NOT NULL
GROUP BY company
ORDER BY avg_star_rating DESC;

Number of Companies w/ More than 5000 Reviews = 40

10.	Add the code to order the query in #9 from highest to lowest average star rating. Which company with more than 5000 reviews across all locations in the dataset has the highest star rating? What is that rating?
SELECT company, ROUND (AVG (star_rating), 1) AS avg_star_rating
FROM data_analyst_jobs
WHERE review_count > 5000 AND company IS NOT NULL
GROUP BY company
ORDER BY avg_star_rating DESC;

Highest Star Rating - Unilever (4.2 Avg. Star Rating)
*Tied w/ General Motors, Nike, American Express, Microsoft, Kaiser Permanente*

11.	Find all the job titles that contain the word ‘Analyst’. How many different job titles are there? 
SELECT COUNT (DISTINCT title)
FROM data_analyst_jobs
WHERE title ILIKE '%Analyst%';

Number of Different Job Titles w/ 'Analyst' = 744

12.	How many different job titles do not contain either the word ‘Analyst’ or the word ‘Analytics’? What word do these positions have in common?
SELECT title, COUNT (DISTINCT title) AS distinct_title
FROM data_analyst_jobs
WHERE title ILIKE '%Analyst%'
	  OR title ILIKE '%Analytics%'
GROUP BY title;

Number of Different Job Titles w/ 'Analyst' OR 'Analytics' = 877
Commonality - Words appear at the beginning and/or the end of a job title (Analyst - role / Analytics - department)
Commonality - Type of analyst / analytics before ('Aircraft Data Analyst', 'Supply Chain Analytics')

**BONUS:**
You want to understand which jobs requiring SQL are hard to fill. Find the number of jobs by industry (domain) that require SQL and have been posted longer than 3 weeks. 
 - Disregard any postings where the domain is NULL. 
 - Order your results so that the domain with the greatest number of `hard to fill` jobs is at the top. 
  - Which three industries are in the top 4 on this list? How many jobs have been listed for more than 3 weeks for each of the top 4?

SELECT domain,
	   COUNT (title) AS title_count
FROM data_analyst_jobs
WHERE skill ILIKE '%SQL%'
	  AND days_since_posting > 21
	  AND domain NOT LIKE 'null'
GROUP BY domain
ORDER BY title_count DESC
LIMIT 4;

1. Internet and Software - 62
2. Banks and Financial Services - 61
3. Consulting and Business Services - 57
4. Health Care - 52