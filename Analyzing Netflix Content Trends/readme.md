# Analyzing Netflix Content Trends

This project explores a **Netflix dataset** to understand content trends, growth, and popularity. Key insights include country associations, top directors, the balance between movies and TV shows, and genre growth rates. The dataset `netflix_titles` contains comprehensive information about movies and TV shows available on Netflix. Columns include: `show_id` (unique identifier), `type` (Movie or TV Show), `title` (name of the content), `director` (director(s)), `cast` (main actors/actresses), `country` (production country), `date_added` (date added to Netflix), `release_year` (year of release), `rating` (age rating), `duration` (duration), `listed_in` (genre(s)), and `description` (brief summary).

**SQL Queries and Analyses:**

Count of Movies vs TV Shows:
SELECT type, COUNT(\*) AS COUNT
FROM netflix\_titles
GROUP BY type;

\-- OR

SELECT
COUNT(DISTINCT CASE WHEN type = 'Movie' THEN show\_id END) AS Movie\_Count,
COUNT(DISTINCT CASE WHEN type = 'TV Show' THEN show\_id END) AS Tv\_ShowCount
FROM netflix\_titles;

Percentage of Content Without a Country:
SELECT
COUNT(*) AS TotalContent,
CAST(COUNT(CASE WHEN country IS NULL THEN 1 END) AS decimal) AS CountryAssociatedCOUNT,
CAST(COUNT(CASE WHEN country IS NULL THEN 1 END) AS decimal)/ CAST(COUNT(*) AS decimal) \*100 AS percentage\_without\_country
FROM netflix\_titles;

Top 3 Directors with the Most Content:
;WITH director\_stats AS
(
SELECT
director,
COUNT(\*) AS titles\_Count,
MAX(release\_year) AS most\_recent\_year
FROM netflix\_titles
WHERE director IS NOT NULL AND director <> ''
GROUP BY director
)
SELECT TOP 3 \*
FROM director\_stats
ORDER BY titles\_Count DESC;

Movie vs TV Show Distribution (2015–2021):
WITH yearly\_count AS
(
SELECT
DATEPART(YEAR, date\_added) AS Year,
type,
COUNT(\*) AS Count
FROM netflix\_titles
WHERE date\_added BETWEEN '2015-01-01' AND '2021-01-01' AND date\_added IS NOT NULL
GROUP BY DATEPART(YEAR, date\_added), type
)
SELECT
Year,
CAST(SUM(CASE WHEN type = 'Movie' THEN Count END) AS numeric) / CAST(SUM(Count) AS numeric) \* 100 AS Movie\_Percentage,
CAST(SUM(CASE WHEN type = 'TV Show' THEN Count END) AS numeric) / CAST(SUM(Count) AS numeric) \* 100 AS TVShow\_Percentage
FROM yearly\_count
GROUP BY Year;

Top 5 Fastest Growing Genres (Average Month-over-Month Growth):
WITH genre\_month AS
(
SELECT
CAST(DATEADD(MONTH, DATEDIFF(MONTH,0,date\_added), 0) AS DATE) AS Month,
VALUE AS Genre,
COUNT(\*) AS monthly\_count
FROM netflix\_titles
CROSS APPLY string\_split(listed\_in, ',')
WHERE date\_added IS NOT NULL
GROUP BY DATEADD(MONTH, DATEDIFF(MONTH, 0, date\_added), 0), VALUE
),
growth\_rate AS
(
SELECT
genre,
Month,
monthly\_count,
LAG(monthly\_count) OVER(PARTITION BY genre ORDER BY genre, Month) AS prev\_month\_count,
(CAST(monthly\_count - LAG(monthly\_count) OVER(PARTITION BY genre ORDER BY genre, Month) AS numeric) / CAST(LAG(monthly\_count) OVER(PARTITION BY genre ORDER BY genre, Month) AS numeric)) \* 100 AS growth
FROM genre\_month
)
SELECT TOP 5
TRIM(genre) AS genre,
AVG(growth) AS avg\_growth
FROM growth\_rate
GROUP BY TRIM(genre)
ORDER BY AVG(growth) DESC;

Key insights include: a significant portion of content does not have a country associated, a few prolific directors dominate the dataset, movie vs TV show distribution varies year by year reflecting content strategy trends, and certain genres are growing faster than others, indicating shifts in viewer preference. Technologies used include SQL Server for querying and analysis, with optional visualization in Excel, Tableau, or Python. This project highlights Netflix content trends and growth patterns, offering insights into production countries, popular directors, content type distributions, and genre growth, which can guide decision-making in content acquisition and recommendation strategies.
