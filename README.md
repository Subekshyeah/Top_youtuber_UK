# Data Portfolio: Excel to Power BI 

# Table of contents 

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Mockup](#mockup)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration](#data-exploration)
  - [Data Cleaning](#data-cleaning)
  - [Transform the Data](#transform-the-data)
  - [Create the SQL View](#create-the-sql-view)
- [Testing](#testing)
  - [Data Quality Tests](#data-quality-tests)
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

- What is the key pain point? 

The Head of Marketing wants to find out who the top YouTubers are in 2024 to decide on which YouTubers would be best to run marketing campaigns throughout the rest of the year.


- What is the ideal solution? 

To create a dashboard that provides insights into the top UK YouTubers in 2024 that includes their 
- subscriber count
- total views
- total videos, and
- engagement metrics

This will help the marketing team make informed decisions about which YouTubers to collaborate with for their marketing campaigns.

## User story 

As the Head of Marketing, I want to use a dashboard that analyses YouTube channel data in the UK . 

This dashboard should allow me to identify the top performing channels based on metrics like subscriber base and average views. 

With this information, I can make more informed decisions about which Youtubers are right to collaborate with, and therefore maximize how effective each marketing campaign is.


# Data source 

- What data is needed to achieve our objective?

We need data on the top UK YouTubers in 2024 that includes their 
- channel names
- total subscribers
- total views
- total videos uploaded



- Where is the data coming from? 
The data is sourced from Kaggle (an Excel extract), [see here to find it.](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download)


# Stages

- Design
- Developement
- Testing
- Analysis 
 


# Design 

## Dashboard components required 
- What should the dashboard contain based on the requirements provided?

To understand what it should contain, we need to figure out what questions we need the dashboard to answer:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

For now, these are some of the questions we need to answer, this may change as we progress down our analysis. 


## Dashboard mockup

- What should it look like? 

Some of the data visuals that may be appropriate in answering our questions include:

1. Table
2. Treemap
3. Scorecards
4. Horizontal bar chart 

## Tools 


| Tool | Purpose |
| --- | --- |
| Excel | Exploring the data |
| MySQL | Cleaning, testing, and analyzing the data |
| Power BI | Visualizing the data via interactive dashboards |
| GitHub | Hosting the project documentation and version control |
| Mokkup AI | Designing the wireframe/mockup of the dashboard | 


# Development

## Pseudocode

- What's the general approach in creating this solution from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into MySQL
4. Clean the data with SQL
5. Test the data with SQL
6. Visualize the data in Power BI
7. Generate the findings based on the insights
8. Write the documentation + commentary
9. Publish the data to GitHub Pages

## Data exploration notes

This is the stage where you have a scan of what's in the data, errors, inconcsistencies, bugs, weird and corrupted characters etc  


- What are your initial observations with this dataset? What's caught your attention so far? 

1. There are at least 4 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data. 
2. The first column contains the channel ID with what appears to be channel IDS, which are separated by a @ symbol - we need to extract the channel names from this.
3. Some of the cells and header names are in a different language - we need to confirm if these columns are needed, and if so, we need to address them.
4. We have more data than we need, so some of these columns would need to be removed





## Data cleaning 
- What do we expect the clean data to look like? (What should it contain? What contraints should we apply to it?)

The aim is to refine our dataset to ensure it is structured and ready for analysis. 

The cleaned data should meet the following criteria and constraints:

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records.

Below is a table outlining the constraints on our cleaned dataset:

| Property | Description |
| --- | --- |
| Number of Rows | 100 |
| Number of Columns | 4 |

And here is a tabular representation of the expected schema for the clean data:

| Column Name | Data Type | Nullable |
| --- | --- | --- |
| channel_name | VARCHAR | NO |
| total_subscribers | INTEGER | NO |
| total_views | INTEGER | NO |
| total_videos | INTEGER | NO |



- What steps are needed to clean and shape the data into the desired format?

1. Remove unnecessary columns by only selecting the ones you need
2. Extract Youtube channel names from the first column
3. Rename columns using aliases







### Transform the data 



```sql
/*
# 1. Select the required columns
# 2. Extract the channel name from the 'NOMBRE' column
*/

-- 1.
SELECT
    SUBSTRING(NOMBRE, 1, LOCATE('@', NOMBRE) - 1) AS channel_name,  -- 2.
    total_subscribers,
    total_views,
    total_videos
FROM
    top_uk_youtubers_2024;
```


### Create the SQL View 

```sql
/*
# 1. Create a view to store the transformed data
# 2. Cast the extracted channel name as VARCHAR(100)
# 3. Select the required columns from the top_uk_youtubers_2024 SQL table 
*/

-- 1.
CREATE VIEW view_uk_youtubers_2024 AS

-- 2.
SELECT
    CAST(SUBSTRING(NOMBRE, 1, LOCATE('@', NOMBRE) - 1) AS CHAR(100)) AS channel_name, -- 2. 
    total_subscribers,
    total_views,
    total_videos

-- 3.
FROM
    top_uk_youtubers_2024;
```


# Testing 

- What data quality and validation checks are you going to create?

Here are the data quality tests conducted:

## Row count check
```sql
/*
# Count the total number of records (or rows) are in the SQL view
*/

SELECT
    COUNT(*) AS no_of_rows
FROM
    view_uk_youtubers_2024;
```


## Column count check
### SQL query 
```sql
/*
# Count the total number of columns (or fields) are in the SQL view
*/

SELECT
    COUNT(*) AS column_count
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';
```
### Output 


## Data type check
### SQL query 
```sql
/*
# Check the data types of each column from the view by checking the INFORMATION SCHEMA view
*/

-- 1.
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';
```
### Output

## Duplicate count check
### SQL query 
```sql
/*
# 1. Check for duplicate rows in the view
# 2. Group by the channel name
# 3. Filter for groups with more than one row
*/

-- 1.
SELECT
    channel_name,
    COUNT(*) AS duplicate_count
FROM
    view_uk_youtubers_2024

-- 2.
GROUP BY
    channel_name

-- 3.
HAVING
    COUNT(*) > 1;
```
### Output
# Visualization 


## Results

- What does the dashboard look like?

![GIF of Power BI Dashboard](assets/images/PowerBIdashboard.png)

This shows the Top UK Youtubers in 2024 so far. 


## DAX Measures

### 1. Total Subscribers (M)
```sql
Total Subscribers (M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(sumOfSubscribers,million)

RETURN totalSubscribers
```

### 2. Total Views (B)
```sql
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = ROUND(sumOfTotalViews / billion, 2)

RETURN totalViews
```

### 3. Total Videos
```sql
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

RETURN totalVideos
```

### 4. Average Views Per Video (M)
```sql
Average Views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR  avgViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())

RETURN finalAvgViewsPerVideo 
```


### 5. Subscriber Engagement Rate
```sql
Subscriber Engagement Rate = 
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())

RETURN subscriberEngRate 
```


### 6. Views per subscriber
```sql
Views Per Subscriber = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews, sumOfTotalSubscribers, BLANK())

RETURN viewsPerSubscriber 
```




# Analysis 

## Findings

- What did we find?

For this analysis, we're going to focus on the questions below to get the information we need for our marketing client - 

Here are the key questions we need to answer for our marketing client: 
1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?


### 1. Who are the top 10 YouTubers with the most subscribers?

| Rank | Channel Name | Subscribers (M) |
|------|--------------|-----------------|
| 1    | NoCopyrightSounds | 34.10 |
| 2    | DanTDM | 29.20 |
| 3    | Dan Rhodes | 26.60 |
| 4    | Miss Katy | 25.50 |
| 5    | Mister Max | 25.10 |
| 6    | KSI Music | 25.00 |
| 7    | Dua Lipa | 24.40 |
| 8    | Jelly | 23.40 |
| 9    | Mrwhosetheboss | 21.60 |
| 10   | Ali-A | 19.70 |


### 2. Which 3 channels have uploaded the most videos?

| Rank | Channel Name | Videos Uploaded |
|------|--------------|-----------------|
| 1    | GRM Daily | 15,448 |
| 2    | Man City | 9,638 |
| 3    | YOGSCAST Lewis & Simon | 7,317 |



### 3. Which 3 channels have the most views?

| Rank | Channel Name | Total Views (B) |
|------|--------------|-----------------|
| 1    | DanTDM | 20.24 |
| 2    | Dan Rhodes | 19.21 |
| 3    | Mister Max | 16.55 |


### 4. Which 3 channels have the highest average views per video?

| Rank | Channel Name | Average Views per Video (M) |
|------|--------------|----------------------------|
| 1    | Mark Ronson | 361.06 |
| 2    | Jessie J | 26.01 |
| 3    | The Beatles | 22.07 |


### 5. Which 3 channels have the highest views per subscriber ratio?

| Rank | Channel Name | Views per Subscriber |
|------|--------------|---------------------|
| 1    | GRM Daily | 1,220.94 |
| 2    | Nickelodeon UK | 1,069.27 |
| 3    | Disney Kids | 1,607.07 |



### 6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

| Rank | Channel Name | Subscriber Engagement Rate |
|------|--------------|---------------------------|
| 1    | Mark Ronson | 357,500 |
| 2    | Jessie J | 45,188.28 |
| 3    | Dua Lipa | 76,972.56 |




### Notes

For this analysis, we'll prioritize analysing the metrics that are important in generating the expected ROI for our marketing client, which are the YouTube channels with the most:

- subscribers
- total views
- videos uploaded



## Validation 

### 1. Youtubers with the most subscribers 

#### Calculation breakdown

Campaign idea = product placement 

a. NoCopyrightSounds 
- Average views per video = 5.97 million
- Product cost = $5
- Potential units sold per video = 5.97 million x 2% conversion rate = 119,400 units sold
- Potential revenue per video = 119,400 x $5 = $597,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $597,000 - $50,000 = $547,000**

b. DanTDM
- Average views per video = 5.37 million
- Product cost = $5
- Potential units sold per video = 5.37 million x 2% conversion rate = 107,400 units sold
- Potential revenue per video = 107,400 x $5 = $537,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $537,000 - $50,000 = $487,000**

c. Dan Rhodes
- Average views per video = 11.32 million
- Product cost = $5
- Potential units sold per video = 11.32 million x 2% conversion rate = 226,400 units sold
- Potential revenue per video = 226,400 x $5 = $1,132,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $1,132,000 - $50,000 = $1,082,000**

Best option from category: Dan Rhodes


#### SQL query 

```sql
/* 
# 1. Define variables 
# 2. Create a CTE that rounds the average views per video 
# 3. Select the columns you need and create calculated columns from existing ones 
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/

-- 1. 
SET @conversionRate = 0.02;        -- The conversion rate @ 2%
SET @productCost = 5.0;            -- The product cost @ $5
SET @campaignCost = 50000.0;       -- The campaign cost @ $50,000

-- 2.  
WITH ChannelData AS (
    SELECT 
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS DECIMAL(20,2)) / total_videos), -4) AS rounded_avg_views_per_video
    FROM 
        view_uk_youtubers_2024
)

-- 3. 
SELECT 
    channel_name,
    rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    ((rounded_avg_views_per_video * @conversionRate * @productCost) - @campaignCost) AS net_profit
FROM 
    ChannelData

-- 4. 
WHERE 
    channel_name IN ('NoCopyrightSounds', 'DanTDM', 'Dan Rhodes')    

-- 5.  
ORDER BY
    net_profit DESC;
```

#### Output

![Most subscribers](assets/images/youtubers_with_the_most_subs.png)

### 2. Youtubers with the most videos uploaded

#### Calculation breakdown 

Campaign idea = sponsored video series  

a. GRM Daily
- Average views per video = 525,781
- Product cost = $5
- Potential units sold per video = 525,781 x 2% conversion rate = 10,516 units sold
- Potential revenue per video = 10,516 x $5 = $52,580
- Campaign cost (11-videos @ $5,000 each) = $55,000
- **Net profit = $52,580 - $55,000 = -$2,420 (potential loss)**

b. Man City
- Average views per video = 275,026
- Product cost = $5
- Potential units sold per video = 275,026 x 2% conversion rate = 5,501 units sold
- Potential revenue per video = 5,501 x $5 = $27,505
- Campaign cost (11-videos @ $5,000 each) = $55,000
- **Net profit = $27,505 - $55,000 = -$27,495 (potential loss)**

c. YOGSCAST Lewis & Simon
- Average views per video = 637,095
- Product cost = $5
- Potential units sold per video = 637,095 x 2% conversion rate = 12,742 units sold
- Potential revenue per video = 12,742 x $5 = $63,710
- Campaign cost (11-videos @ $5,000 each) = $55,000
- **Net profit = $63,710 - $55,000 = $8,710 (profit)**

Best option from category: YOGSCAST Lewis & Simon

#### SQL query 
```sql
/* 
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/

-- 1.
SET @conversionRate = 0.02;              -- The conversion rate @ 2%
SET @productCost = 5.0;                  -- The product cost @ $5
SET @campaignCostPerVideo = 5000.0;      -- The campaign cost per video @ $5,000
SET @numberOfVideos = 11;                -- The number of videos (11)

-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS DECIMAL(20,2)) / total_videos), -4) AS rounded_avg_views_per_video
    FROM
        view_uk_youtubers_2024
)

-- 3.
SELECT
    channel_name,
    rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    ((rounded_avg_views_per_video * @conversionRate * @productCost) - (@campaignCostPerVideo * @numberOfVideos)) AS net_profit
FROM
    ChannelData

-- 4.
WHERE
    channel_name IN ('GRM Daily', 'Man City', 'YOGSCAST Lewis & Simon')

-- 5.
ORDER BY
    net_profit DESC;
```

#### Output

![Most videos](assets/images/youtubers_with_the_most_videos.png)


### 3. Youtubers with the most views 

#### Calculation breakdown

Campaign idea = Influencer marketing 

a. DanTDM
- Average views per video = 5.37 million
- Product cost = $5
- Potential units sold per video = 5.37 million x 2% conversion rate = 107,400 units sold
- Potential revenue per video = 107,400 x $5 = $537,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $537,000 - $130,000 = $407,000**

b. Dan Rhodes
- Average views per video = 11.32 million
- Product cost = $5
- Potential units sold per video = 11.32 million x 2% conversion rate = 226,400 units sold
- Potential revenue per video = 226,400 x $5 = $1,132,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $1,132,000 - $130,000 = $1,002,000**

c. Mister Max
- Average views per video = 13.65 million
- Product cost = $5
- Potential units sold per video = 13.65 million x 2% conversion rate = 273,000 units sold
- Potential revenue per video = 273,000 x $5 = $1,365,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $1,365,000 - $130,000 = $1,235,000**

Best option from category: Mister Max

#### SQL query 
```sql
/* 
# 1. Define variables 
# 2. Create a CTE that rounds the average views per video 
# 3. Select the columns you need and create calculated columns from existing ones 
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/

-- 1. 
SET @conversionRate = 0.02;        -- The conversion rate @ 2%
SET @productCost = 5.0;            -- The product cost @ $5
SET @campaignCost = 130000.0;      -- The campaign cost @ $130,000

-- 2.  
WITH ChannelData AS (
    SELECT 
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS DECIMAL(20,2)) / total_videos), -4) AS rounded_avg_views_per_video
    FROM 
        view_uk_youtubers_2024
)

-- 3. 
SELECT 
    channel_name,
    rounded_avg_views_per_video,
    ROUND((rounded_avg_views_per_video * @conversionRate)) AS potential_units_sold_per_video,
    ROUND((rounded_avg_views_per_video * @conversionRate * @productCost)) AS potential_revenue_per_video,
    ROUND((rounded_avg_views_per_video * @conversionRate * @productCost) - @campaignCost) AS net_profit
FROM 
    ChannelData

-- 4. 
WHERE 
    channel_name IN ('DanTDM', 'Dan Rhodes', 'Mister Max')

-- 5.  
ORDER BY
    net_profit DESC;
```

#### Output

![Most views](assets/images/youtubers_with_the_most_views.png)



## Discovery

- What did we learn?

We discovered that:

1. NoCopyrightSounds, DanTDM and Dan Rhodes are the channels with the most subscribers in the UK
2. GRM Daily, Man City and YOGSCAST Lewis & Simon are the channels with the most videos uploaded
3. DanTDM, Dan Rhodes and Mister Max are the channels with the most views
4. Entertainment channels are useful for broader reach, as the channels posting consistently on their platforms and generating the most engagement are focused on entertainment and music 




## Recommendations 

- What do you recommend based on the insights gathered? 
  
1. Dan Rhodes is the best YouTube channel to collaborate with if we want to maximize visibility because this channel has the most YouTube subscribers in the UK
2. Although GRM Daily, Man City and YOGSCAST Lewis & Simon are regular publishers on YouTube, it may be worth considering whether collaborating with them with the current budget caps are worth the effort, as the potential return on investments is significantly lower compared to the other channels.
3. Mister Max is the best YouTuber to collaborate with if we're interested in maximizing reach, but collaborating with DanTDM and Dan Rhodes may be better long-term options considering the fact that they both have large subscriber bases and are averaging significantly high number of views.
4. The top 3 channels to form collaborations with are Dan Rhodes, DanTDM and Mister Max based on this analysis, because they attract the most engagement on their channels consistently.


### Potential ROI 
- What ROI do we expect if we take this course of action?

1. Setting up a collaboration deal with Dan Rhodes would make the client a net profit of $1,082,000 per video
2. An influencer marketing contract with Mister Max can see the client generate a net profit of $1,235,000
3. If we go with a product placement campaign with DanTDM, this could generate the client approximately $487,000 per video. If we advance with an influencer marketing campaign deal instead, this would make the client a one-off net profit of $407,000.
4. NoCopyrightSounds could profit the client $547,000 per video too (which is worth considering) 




### Action plan
- What course of action should we take and why?

Based on our analysis, we believe the best channel to advance a long-term partnership deal with to promote the client's products is the Dan Rhodes channel. 

We'll have conversations with the marketing client to forecast what they also expect from this collaboration. Once we observe we're hitting the expected milestones, we'll advance with potential partnerships with DanTDM, Mister Max and NoCopyrightSounds channels in the future.   

- What steps do we take to implement the recommended decisions effectively?

1. Reach out to the teams behind each of these channels, starting with Dan Rhodes
2. Negotiate contracts within the budgets allocated to each marketing campaign
3. Kick off the campaigns and track each of their performances against the KPIs
4. Review how the campaigns have gone, gather insights and optimize based on feedback from converted customers and each channel's audiences 


# Conclusion

This analysis has provided valuable insights into the top UK YouTubers for 2024, enabling data-driven decisions for marketing campaign investments. The key takeaways are:

- **Dan Rhodes** offers the best ROI for product placement campaigns
- **Mister Max** maximizes reach for influencer marketing campaigns
- **DanTDM** provides a solid balance of subscribers and engagement
- Channels with the most videos uploaded may not always provide the best ROI

By following the recommended action plan and starting with Dan Rhodes, the marketing team can strategically allocate their budget to maximize campaign effectiveness and profitability.
