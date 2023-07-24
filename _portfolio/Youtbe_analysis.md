---
title: "Youtube video statistics analyisis"
excerpt: "The goal of this project is to analyze the trending videos from youtube to understanding what makes a video start trending. Then once the vdeo features are analyzed, a ML model is build that will take a video title and predict how well it will perform.  "
collection: portfolio
---

All the code can be found here:

## Table Of Contents
  
  - Introduction
  - About the Dataset
  - Project outline
  - Summary of EDA process
  - Summary of Hypothesis testing
  - Conclusion of project

## About the dataset

The dataset contains 199237 rows and 13 columns. The features are as follows:
- title: title of the video                 
- publishedAt: date when the video was posted
- channelTitle: youtube channel that posted the video            
- categoryName: the video category            
- trending_date: the date the video was trending           
- tags: the tags on the video                    
- view_count: total views              
- likes: total likes the video received                   
- dislikes: total dislikes the video received                
- comment_count: total comments the video received           
- comments_disabled: whether the comments were disabled       
- ratings_disabled: whether the likes were disabled
- description: the video description 

Data is gathered from Kaggle website and considered as primary data. It can be download from [here](https://www.kaggle.com/datasets/rsrishav/youtube-trending-video-dataset)

## Project outline:
1) Check the complete EDA of YouTube Data [Jupyter Notebook](https://github.com/ALvee-611/YouTube-Analysis/blob/master/Analysis/EDA.ipynb): The dataset are combined, cleaned, wrangled and explored here.

2) Check the complete Hypothesis Testing [Jupyter Notebook](https://github.com/ALvee-611/YouTube-Analysis/blob/master/Analysis/statistical_tests.ipynb): Some of the insights found in the EDA step are tested here. This step also ends with some data cleaning.

<!-- 3) Check the complete Model Building here (in-progress/final stage) [Jupyter Notebook](https://github.com/ALvee-611/YouTube-Analysis/blob/master/Analysis/model_building.ipynb)
4) Check the code of ML model by using this [script]() 
5) Use the [Dashboard]() -->

## Summary of EDA process:
The earliest and latest videos in the dataset were published on 2020-07-27 21:49:32+00:00 and 2023-04-30 16:48:52+00:00, respectively. On average, videos take around 5 days and 16 hours to start trending, with the longest time being 34 days. Most videos are published in the evening. There are 14 video categories, with Entertainment, Gaming, Sports, Music, and People & Blogs being the most popular. Music videos tend to receive more likes, comments, and views than other categories. The NBA youtube channel has the most likes, views, and comments, while BTS (방탄소년단) 'Butter' Official MV was the most liked, viewed, and commented video. Likes, views, and comments have a strong positive correlation with each other. Most videos have 0-30 tags, and videos with 10-25 words in the title receive the most views. 40 to 55 tags seem to have good engaement rate compared to other.

## Summary of Hypothesis testing:
Using the non-parametric Kruskal-Wallis test, I found that there is significant difference in the number of views across different video categories. In the EDA phase, I saw that around 10 to 25 words seem to have more views in genral than longer video titles. So I tested if the length of a video title have an effect on the engagement rate using A/B/C test and since the data was not normally distributed I used Kruskal-Wallis test again. Firstly, I found that at least one group is different from the others in terms of engagement rate. Then to check which test had a significantly higher engagement rate, I used pairwise Wilcoxon rank-sum tests with Bonferroni correction.I found that video titles upto 25 words will have similar engagement rate but titles longer than 25 words will tend to have lower engagement rate. I used Chi-Sq test to conclude that there is a significant relationship between video category and time of day the video was posted. I also found significant difference in the number of days a video trends for across different video categories. Finally, I used regression analysis to determine the factors that impact the engagement of a video.

## Conclusion of project

1) The EDA process consisted of mining the data for insights

2) The Hypothesis testing step was used to confirm the findings in the EDA step.
