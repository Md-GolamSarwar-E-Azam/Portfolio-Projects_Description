# Analyzing Netflix Content Trends

This project explores a **Netflix dataset** to understand content trends, growth, and popularity. The dataset `netflix_titles` contains comprehensive information about movies and TV shows available on Netflix.

**Dataset Description:**

* `show_id`: Unique identifier for each title
* `type`: Category of the title (Movie or TV Show)
* `title`: Name of the movie or TV show
* `director`: Director(s) of the title
* `cast`: Main actors or actresses
* `country`: Country where the content was produced
* `date_added`: Date when the title was added to Netflix
* `release_year`: Year the content was released
* `rating`: Age rating of the title
* `duration`: Duration of the title
* `listed_in`: Genre(s) of the title
* `description`: Brief summary of the title

**SQL Queries and Analyses:**

**Count of Movies vs TV Shows:**

```sql
SELECT type, COUNT(*) AS COUNT
FROM netflix_titles
GROUP BY type;

-- OR

SELECT
COUNT(DISTINCT CASE WHEN type = 'Movie' THEN show_id END) AS Movie_Count,
COUNT(DISTINCT CASE WHEN type = 'TV Show' THEN show_id END) AS Tv_ShowCount
FROM netflix_titles;
```

**Percentage of Content Without a Country:**

```sql
SELECT
COUNT(*) AS TotalContent,
CAST(COUNT(CASE WHEN country IS NULL THEN 1 END) AS decimal) AS CountryAssociatedCOUNT,
CAST(COUNT(CASE WHEN country IS NULL THEN 1 END) AS decimal)/ CAST(COUNT(*) AS decimal) *100 AS percentage_without_country
FROM netflix_titles;
```

**Top 3 Directors with the Most Content:**

```sql
;WITH director_stats AS
(
  SELECT
    director,
    COUNT(*) AS titles_Count,
    MAX(release_year) AS most_recent_year
  FROM netflix_titles
  WHERE director IS NOT NULL AND director <> ''
  GROUP BY director
)
SELECT TOP 3 *
FROM director_stats
ORDER BY titles_Count DESC;
```

**Movie vs TV Show Distribution (2015–2021):**

```sql
WITH yearly_count AS
(
  SELECT
    DATEPART(YEAR, date_added) AS Year,
    type,
    COUNT(*) AS Count
  FROM netflix_titles
  WHERE date_added BETWEEN '2015-01-01' AND '2021-01-01' AND date_added IS NOT NULL
  GROUP BY DATEPART(YEAR, date_added), type
)
SELECT
  Year,
  CAST(SUM(CASE WHEN type = 'Movie' THEN Count END) AS numeric) / CAST(SUM(Count) AS numeric) * 100 AS Movie_Percentage,
  CAST(SUM(CASE WHEN type = 'TV Show' THEN Count END) AS numeric) / CAST(SUM(Count) AS numeric) * 100 AS TVShow_Percentage
FROM yearly_count
GROUP BY Year;
```

**Top 5 Fastest Growing Genres (Average Month-over-Month Growth):**

```sql
WITH genre_month AS
(
  SELECT
    CAST(DATEADD(MONTH, DATEDIFF(MONTH,0,date_added), 0) AS DATE) AS Month,
    VALUE AS Genre,
    COUNT(*) AS monthly_count
  FROM netflix_titles
  CROSS APPLY string_split(listed_in, ',')
  WHERE date_added IS NOT NULL
  GROUP BY DATEADD(MONTH, DATEDIFF(MONTH, 0, date_added), 0), VALUE
),
growth_rate AS
(
  SELECT
    genre,
    Month,
    monthly_count,
    LAG(monthly_count) OVER(PARTITION BY genre ORDER BY genre, Month) AS prev_month_count,
    (CAST(monthly_count - LAG(monthly_count) OVER(PARTITION BY genre ORDER BY genre, Month) AS numeric) / CAST(LAG(monthly_count) OVER(PARTITION BY genre ORDER BY genre, Month) AS numeric)) * 100 AS growth
  FROM genre_month
)
SELECT TOP 5
  TRIM(genre) AS genre,
  AVG(growth) AS avg_growth
FROM growth_rate
GROUP BY TRIM(genre)
ORDER BY AVG(growth) DESC;
```

**Key Insights:**

* A significant portion of content does not have a country associated.
* A few prolific directors dominate the dataset.
* Movie vs TV show distribution varies year by year, reflecting content strategy trends.
* Certain genres are growing faster than others, indicating shifts in viewer preference.

This project highlights Netflix content trends and growth patterns, offering insights into production countries, popular directors, content type distributions, and genre growth, which can guide decision-making in content acquisition and recommendation strategies.
