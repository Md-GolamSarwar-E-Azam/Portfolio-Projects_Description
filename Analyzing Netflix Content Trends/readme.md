# 🎬 Netflix Content Analysis

An exploratory data analysis project focused on understanding content trends, growth, and popularity on Netflix using a provided dataset. The project leverages **SQL** to query and analyze the data, with the goal of uncovering key insights into the platform's content library.

## 📊 Key Findings

* **Content Distribution:** An analysis of the dataset revealed the balance between movies and TV shows, highlighting which content type dominates the platform's library.
* **Geographical Association:** The project identified the percentage of content entries that lack a country association, which is a key data quality metric.
* **Top Directors:** The top 3 directors with the most content on Netflix were identified, along with the release year of their most recent title.
* **Content Growth Over Time:** The percentage of movies versus TV shows added annually from 2015 to 2021 was calculated to visualize how Netflix's content strategy has evolved over the years.
* **Genre Trends:** The project determined the average month-over-month growth rate for different genres and pinpointed the top 5 fastest-growing genres on the platform.

## ⚙️ Project Structure

The project is structured around a series of SQL queries designed to answer specific questions about the Netflix dataset.

* `Netflix_Movies_&_Shows.sql`: This file contains all the SQL code used for the analysis, with each query addressing a specific question outlined in the "Key Findings" section.

## 📦 Dataset

The analysis is based on a `netflix_titles` dataset, which contains detailed information about movies and TV shows, including:
* `show_id`: Unique identifier for each title.
* `type`: Category (Movie/TV Show).
* `title`: The name of the content.
* `director`: The director of the content.
* `country`: Country of production.
* `date_added`: Date the title was added to Netflix.
* `release_year`: The original release year.
* `rating`: Age rating.
* `duration`: Length of the content.
* `listed_in`: Genres.
* `description`: A brief summary.

## ✨ How to Use

To run the queries, simply execute the code in the `Netflix_Movies_&_Shows.sql` file on a SQL environment connected to the `netflix_titles` dataset. The code is well-commented to explain the purpose of each query and its logic.
