# Data porfolio

This is my porfolio website!


# Table of contents 

- [Objective](#objective)
- [Data Source](#data-source)
- [Steps of the project](#Steps-of-the-project)
- [Design](#design)
  - [Dashboard Mockup](#Dashboard-Mockup)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode: Steps of the developement section](#Pseudocode: Steps of the developement section)
  - [Data Exploration](#data-exploration)
  - [Data Cleaning, Transfomration and Quality Check](#Data-Cleaning,-Transfomration-and-Quality-Check)
  - [Data Cleaning](#Data-Cleaning)
  - [Create the SQL View](#create-the-sql-view)
- [Testing and Quality Checks](#Testing-and-Quality-Checks)
- [Visualization](#visualization)
  - [Results](#results)
  - [DAX Measures](#dax-measures)
- [Analysis](#analysis)
  - [Findings](#findings)
  - [Validation](#validation)
  - [Discovery](#discovery)
- [Recommendations](#recommendations)
  - [Potential ROI](#potential-roi)
  - [Potential Courses of Actions](#potential-courses-of-actions)
- [Conclusion](#conclusion)





# Objective

- What is the main objective of this project ?
  
  An American Company wants to run an advertisment campaign through Youtube in UK in order to penetrate a new market. The Marketing Team want to find the best UK Youtuber in 2024 they can work with in 2024.

- What is the decision making support ?

  The best solution to support the stakeholders is to create a dashboard that provides strong insight about UK Youtubers:
  
  - Total subscribers
  - Total views
  - Total videos
  - Viewer engagement
  - etc.
  

## User Assignement
  As the Head of the Marketing Team, I want to have access to a dashboard that include the best insights in order to collaborate with the best UK Youtuber in 2024.
  This dashboard should include strong metrics like the average views, subscribers engagement and many more. I want to choose the best performing UK Youtuber to collaborate with to penetrate the UK market.

# Data Source

  The data is coming from a CSV file sourced from Kaggle. Url: [https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download].

# Steps of the project

  - Design the dashboard
  - Developement (Explore data, Clean data and Validate data)
  - Testing
  - Vizualisation
  - Analyzing data
  - Recommendation

# Design 

## Dashboard requirements
  - What are the dashboard requirements based on the information provided:

    1. Who are the top 10 YouTubers with the most subscribers?
    2. Which 3 channels have uploaded the most videos?
    3. Which 3 channels have the most views?
    4. Which 3 channels have the highest average views per video?
    5. Which 3 channels have the highest views per subscriber ratio?
    6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

## Dashboard Mockup

  - Based on the information provided we need to create a dashboard with the following visuals:

|Visuals|
|---|
|A Table with the most important information|
|A Treemap|
|Scorecard with strong insights|
|An horizontal bar chart|

## Tools

  Which tool will be used to complete this project?

| Tools | Purpose |
| --- | --- |
| Excel | To explore data |
| SQL Server | To clean, validate and analyze the data |
| Power Bi | To create an interactive visualization |
| Google Sheets | To manipulate and calculate information from insights |
| Github | To host and present the project documentation |

# Developement

## Pseudocode: Steps of the developement section

  - This are the main steps that will be followed in order to complete the project:

    1. Load data to Excel.
    2. Explore data in Excel.
    3. Load data into SQL Server.
    4. Clean data with SQL.
    5. Test data with SQL.
    6. Load data into Power Bi.
    7. Visualize data in Power Bi.
    8. Compare Power Bi insights with Excel insights done with calculation.
    9. Provide a recommendation and final conclusions based on insights.

## Data Exploration
  - In this section, we will take a look at the raw dataset by using Excel. We will identify the errors, missplealled words, weird characters, etc.

  - During the data exploration we identified the followings:
    1. We observe that some columns names might be in a different language.
    2. Some of the columuns are important for our project.
    3. It seems that the channel id and channel name is separated with an @.
    4. Some of the columns present in the dataset are unessesary for our project.
   
## Data Cleaning, Transfomration and Quality Check
  - SQL Server Management (or SSMS) is used un order to clean data, transform and run quality checks.

  - This are the step to follow in order to clean the data properly for our project:
    1. Select only the columns that are necessary for the project.
    2. Extract the channel name from the first column.
    3. Rename the column names.
    4. Do a row check to verify if there's 100 rows of data.
    5. Do a column check id there'is 4 columns fields.
    6. Do a data type check if the correct data type is assign to the correct field.
    7. Do a null check. It's important to have a complete dataset without any null data.

### Data Cleaning

#### Select columns
```sql
/*

--Selecting colums we need for the project

		select 
			NOMBRE,
			total_subscribers,
			total_views,
			total_videos
		from 
			top_uk_youtubers_2024;
```

### Extracting Youtube Channel Names

```sql
/*

--Extracting Youtube Channels

		select
			CHARINDEX('@', NOMBRE),
			NOMBRE
		from
			top_uk_youtubers_2024;
```

### Create a SQL View 
```sql
/*
1. Renaming Youtube channels colum name and creating a view with all data cleaned

		CREATE VIEW view_uk_youtubers_2024 AS
		SELECT 
			CAST(SUBSTRING(NOMBRE,1,CHARINDEX('@', NOMBRE)-1) AS varchar(100)) AS channel_name,
			total_subscribers,
			total_views,
			total_videos
		FROM
			top_uk_youtubers_2024;
```

## Testing and Quality Checks

### Row count test
```sql
/*

--Row count test

select 
	COUNT(*) as numbers_of_rows
from 
	top_uk_youtubers_2024;

```

### Column count test
```sql
/*

--Column count test

select 
	count(*) as columns_count
from 
	INFORMATION_SCHEMA.COLUMNS
where 
	table_name = 'view_uk_youtubers_2024';
```

### Data Type test
```sql
/*

--Data type test

select 
	COLUMN_NAME,
	DATA_TYPE
from 
	INFORMATION_SCHEMA.COLUMNS
where 
	table_name = 'view_uk_youtubers_2024';
```

### Duplicates Check
```sql
/*

--Duplicates check

select 
	channel_name,
	count(*) as duplicate_count
from 
	view_uk_youtubers_2024
group by
	channel_name
having
	Count(*) > 1
```

# Visualization

## Results 

 - This shows how the final dashboard looks like.

![PNG of PowerBi Dashboard](assets/images/Top UK Youtubers 2024.PNG)

## DAX Measures

### 1.Total subscribers (M)
```sql
/*
Total Subscribers (M) = 
VAR million = 1000000
VAR SumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(SumOfSubscribers, million)

RETURN totalSubscribers
```

### 2. Total Views (B)
```sql
/*
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = DIVIDE(sumOfTotalViews,billion)

RETURN totalViews
```

### 3. Total Videos
```sql
/*
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

RETURN totalVideos
```
### 4. Average Views Per Video (M)
```sql
/*
Avg views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR avgOfViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgOfViewsPerVideo,1000000,BLANK())

RETURN finalAvgViewsPerVideo
```
### 5. Subscribers Engagement
```sql
/*
Subscriber Engagement Rate = 
VAR sumOftotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SuM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngagementRate = DIVIDE(sumOftotalSubscribers,sumOfTotalVideos, BLANK())

RETURN subscriberEngagementRate
```
### 6. Views Per Subscribers 
```sql
/*
Views per Subscriber = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews,sumOfTotalSubscribers, BLANK())

RETURN viewsPerSubscriber
```

# Analysis

## Insights findings

For this analysis, our client asked us to anwsers the following questions :

-Who are the top 10 YouTubers with the most subscribers?
-Which 3 channels have uploaded the most videos?
-Which 3 channels have the most views?
-Which 3 channels have the highest average views per video?
-Which 3 channels have the highest views per subscriber ratio?
-Which 3 channels have the highest subscriber engagement rate per video uploaded?
 

 ### 1.Top 10 Youtubers with the most subscribers
 
 |Position|Channel Name|Total Subscribers (M)|
 |---|---|---|
 |1|NoCopyrightSounds|33.60|
 |2|DanTDM|28.60|
 |3|Dan Rhodes|26.50|
 |4|Miss Katy|24.50|
 |5|Mister Max|24.40|
 |6|KSI|24.10|
 |7|Jelly|23.50|
 |8|Dua Lipa|23.30|
 |9|Sidemen|21.00|
 |10|Ali-A|18.90|
 
 ### 2.Top 3 Youtube channels with the most uploaded videos

 |Position|Channel Name|Total videos|
 |---|---|---|
 |1|24 News HD|16,5103|
 |2|Sky News|46,009|
 |3|BBC News ????|40,179|
 
 ### 3.Top 3 Youtube channels with the most views

 |Position|Channel Name|Total Views|
 |---|---|---|
 |1|DanTDM|19.78|
 |2|Dan Rhodes|18.56|
 |3|Mister Max|16.97|
 
 ### 4.Top 3 Youtube channels with the highest average views per video

 |Position|Channel Name|Average Views Per Video (M)|
 |---|---|---|
 |1|Mark Ronson|322.79|
 |2|Jessie J|59.77|
 |3|Dua Lipa|57.62|

 ### 5.Top 3 Youtube channels with the highest views per subscribers ratio

 |Position|Channel Name|Average Views Per Subscribers Ratio|
 |---|---|---|
 |1|GRM Daily|1185.79|
 |2|Nickelodeon UK|1061.04|
 |3|Disney Junior UK|1031.97|
 
 ### 6.Top 3 Youtube channels with the highest subscriber engagement rate per video uploaded

 |Position|Channel Name|Subscriber Engagement Rate|
 |---|---|---|
 |1|Mark Ronson|343,000|
 |2|Jessie J|110,416.67|
 |3|Dua Lipa|104,954.95|

 ## Validation

	- For this analysis, we will analyze the Youtube channels with the most subscribers to determine de portential ROI for our client.

 ### Potential ROI for the Youtube channels with the most subscibers.

 #### Variables

 |Variables|Value|
 |---|---|
 |Conversion Rate|2%|
 |Product Cost|5$|
 |Campaign Cost|50,000$|

 ##### Breakdown calculation

 ##### 1. NoCopyrightSounds

 Average views = 6,920,000
 Potential Product Sales per Video = 6,920,000*2% = 138,400 sales
 Potential Revenue per video = 138,400*5$ = 692,000$
 Net Profit = 692,000 - 50,000 = 642,000$

 

 
 
 	






    


