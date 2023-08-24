---
title: "Case Study #5 - Data Mart (8 week SQL Challenge)"
excerpt: "The key business question we want to answer are the following:
    1.What was the quantifiable impact of the changes introduced in June 2020?
    
    2. Which platform, region, segment and customer types were the most impacted by this change?
    
    3. What can we do about future introduction of similar sustainability updates to the business to minimise impact on sales?
"
collection: portfolio
---
![main_image](..\..\images\second_images\case_study_week_5.png)

Problem Statement:

Data Mart is Danny’s latest venture and after running international operations for his online supermarket that specialises in fresh produce - Danny is asking for your support to analyse his sales performance.

In June 2020 - large scale supply changes were made at Data Mart. All Data Mart products now use sustainable packaging methods in every single step from the farm all the way to the customer.

Danny needs your help to quantify the impact of this change on the sales performance for Data Mart and it’s separate business areas.

Solution:

This is a case study and is part of a 8 part series of 8 different case studies that focus on different aspects of a business.

In this case study we will answer the following:

* What was the quantifiable impact of the changes introduced in June 2020?
    
* Which platform, region, segment and customer types were the most impacted by this change?
    
* What can we do about future introduction of similar sustainability updates to the business to minimise impact on sales?

The database schema and information about the data attributes can be found here: <a href="https://8weeksqlchallenge.com/case-study-5/" target="_blank"></a>.

## Data Cleaning:

We will need to clean up the data a bit before we can proceed with solving it.

```sql
SET
  search_path = data_mart;
SELECT
  date AS week_date,
  EXTRACT(
    WEEK
    FROM
      date
  ) :: int AS week_number,
  EXTRACT(
    MONTH
    FROM
      date
  ) :: int AS month_number,
  EXTRACT(
    YEAR
    FROM
      date
  ) :: int AS calendar_year,
  region,
  platform,
  CASE
    WHEN segment = 'null' then 'unknown'
    ELSE segment
  END AS segment,
  CASE
    WHEN substr(segment, 2, 1) = '1' then 'Young Adults'
    WHEN substr(segment, 2, 1) = '2' then 'Middle Aged'
    WHEN substr(segment, 2, 1) in ('3', '4') then 'Retirees'
    ELSE 'unknown'
  END AS age_band,
  CASE
    WHEN substr(segment, 1, 1) = 'C' then 'Couples'
    WHEN substr(segment, 1, 1) = 'F' then 'Families'
    ELSE 'unknown'
  END AS demographic,
  customer_type,
  transactions,
  sales,
  avg_transaction INTO clean_weekly_sales
FROM
  weekly_sales,
  LATERAL (
    SELECT
      TO_DATE(week_date, 'DD/MM/YY') AS date
  ) date,
  LATERAL (
    SELECT
      ROUND((sales :: numeric / transactions), 2) AS avg_transaction
  ) avt
ORDER BY
  calendar_year
```

## 2. Data Exploration

### 1. What day of the week is used for each week_date value?

```sql
SET
  search_path = data_mart;
SELECT
  EXTRACT(
    ISODOW
    FROM
      week_date
  ) AS day_of_week
FROM
  clean_weekly_sales
GROUP BY
  1
```

| day_of_week  |  
|--------------|
| 1            |

Each week starts from Monday in the `week_date` table

### 2. What range of week numbers are missing from the dataset?

We can get a range of the weeks that are in our table:

```sql
SET search_path = data_mart;
SELECT
  week_number
FROM
  clean_weekly_sales
GROUP BY
  1
ORDER BY
  1
```

| week_number    |
|----------------|
| 13             | 
| 14             |
| 15             |
| 16             |
| 17             |
| 18             |
| 19             |
| 20             |
| 21             |
| 22             |
| 23             |
| 24             |
| 25             |
| 26             |
| 27             |
| 28             |
| 29             |
| 30             |
| 31             |
| 32             |
| 33             |
| 34             |
| 35             |
| 36             |


```sql
SET
  search_path = data_mart;
WITH series as (
    SELECT
      GENERATE_SERIES(1, 52) as missing_weeks
    FROM
      clean_weekly_sales
  )
SELECT
  missing_weeks
FROM
  series
WHERE
  missing_weeks NOT IN(
    SELECT
      week_number
    FROM
      clean_weekly_sales
  )
GROUP BY
  1
ORDER BY
  1
```

| missing_weeks  |
|----------------|
| 1              | 
| 2              |
| 3              |
| 4              |
| 5              |
| 6              |
| 7              |
| 8              |
| 9              |
| 10             |
| 11             |
| 12             |
| 37             |
| 38             |
| 39             |
| 40             |
| 41             |
| 42             |
| 43             |
| 44             |
| 45             |
| 46             |
| 47             |
| 48             |
| 49             |
| 50             |
| 51             |
| 52             |

Weeks 1 - 12 and 37 - 52 are missing from the dataset.

### 3. How many total transactions were there for each year in the dataset?

We need to group all the transactions by years and count the total number of transaction using `SUM` function.

```sql
SELECT
  calendar_year,
  SUM(transactions) AS total_transactions
FROM
  clean_weekly_sales
GROUP BY
  1
ORDER BY
  1
```
  
| calendar_year | total_transactions  |
|---------------|-------------------------------|
| 2018          | 346406460                     |
| 2019          | 365639285                     |
| 2020          | 375813651                     |

The number of transactions are increasing year by year.

### 4. What is the total sales for each region for each month?
```sql
SELECT
  region,
  month_number,
  SUM(sales) AS total_sales
FROM
  clean_weekly_sales
GROUP BY
  1, 2
ORDER BY
  1,2
```

| region        | month_number | total_sales  |
|---------------|--------------|--------------|
| AFRICA        | 3            | 567767480    |
| AFRICA        | 4            | 1911783504   |
| AFRICA        | 5            | 1647244738   |
| AFRICA        | 6            | 1767559760   |
| AFRICA        | 7            | 1960219710   |
| AFRICA        | 8            | 1809596890   |
| AFRICA        | 9            | 276320987    |
| ASIA          | 3            | 529770793    |
| ASIA          | 4            | 1804628707   |
| ASIA          | 5            | 1526285399   |
| ASIA          | 6            | 1619482889   |
| ASIA          | 7            | 1768844756   |
| ASIA          | 8            | 1663320609   |
| ASIA          | 9            | 252836807    |
| CANADA        | 3            | 144634329    |
| CANADA        | 4            | 484552594    |
| CANADA        | 5            | 412378365    |
| CANADA        | 6            | 443846698    |
| CANADA        | 7            | 477134947    |
| CANADA        | 8            | 447073019    |
| CANADA        | 9            | 69067959     |
| EUROPE        | 3            | 35337093     |
| EUROPE        | 4            | 127334255    |
| EUROPE        | 5            | 109338389    |
| EUROPE        | 6            | 122813826    |
| EUROPE        | 7            | 136757466    |
| EUROPE        | 8            | 122102995    |
| EUROPE        | 9            | 18877433     |
| OCEANIA       | 3            | 783282888    |
| OCEANIA       | 4            | 2599767620   |
| OCEANIA       | 5            | 2215657304   |
| OCEANIA       | 6            | 2371884744   |
| OCEANIA       | 7            | 2563459400   |
| OCEANIA       | 8            | 2432313652   |
| OCEANIA       | 9            | 372465518    |
| SOUTH AMERICA | 3            | 71023109     |
| SOUTH AMERICA | 4            | 238451531    |
| SOUTH AMERICA | 5            | 201391809    |
| SOUTH AMERICA | 6            | 218247455    |
| SOUTH AMERICA | 7            | 235582776    |
| SOUTH AMERICA | 8            | 221166052    |
| SOUTH AMERICA | 9            | 34175583     |
| USA           | 3            | 225353043    |
| USA           | 4            | 759786323    |
| USA           | 5            | 655967121    |
| USA           | 6            | 703878990    |
| USA           | 7            | 760331754    |
| USA           | 8            | 712002790    |
| USA           | 9            | 110532368    |

We can see that Oceania generates most of the sales, followed by Africa and Asia.

### 5. What is the total count of transactions for each platform?

```sql
SELECT
  platform,
  SUM(transactions) as total_transactions
FROM
  clean_weekly_sales
GROUP BY
  1
ORDER BY
  1
```

| platform | total_transactions  |
|----------|---------------------|
| Retail   | 1081934227          |
| Shopify  | 5925169             |

Retail has more transactions than Shopify.

### 6. What is the percentage of sales for Retail vs Shopify for each month?

```sql
SELECT
  sr.month_number,  sr.calendar_year,  ROUND(100 *(SUM(sales) :: numeric / total_sales_s), 1) AS percentage_of_sales_retail,
  100 - ROUND(100 *(SUM(sales) :: numeric / total_sales_s), 1) AS percentage_of_sales_shopify
FROM
  clean_weekly_sales AS R
  JOIN (
    SELECT
      S.month_number, S.calendar_year, SUM(sales) AS total_sales_s
    FROM
      clean_weekly_sales AS S
    GROUP BY
      S.month_number,
      S.calendar_year
  ) S ON sr.month_number = S.month_number
  and S.calendar_year = R.calendar_year
WHERE
  R.platform = 'Retail'
GROUP BY
  R.month_number, R.calendar_year, total_sales_s
ORDER BY
  2,  1
```

| month_number | calendar_year | percentage_of_sales_retail | percentage_of_sales_shopify  |
|--------------|---------------|----------------------------|------------------------------|
| 3            | 2018          | 97.9                       | 2.1                          |
| 4            | 2018          | 97.9                       | 2.1                          |
| 5            | 2018          | 97.7                       | 2.3                          |
| 6            | 2018          | 97.8                       | 2.2                          |
| 7            | 2018          | 97.8                       | 2.2                          |
| 8            | 2018          | 97.7                       | 2.3                          |
| 9            | 2018          | 97.7                       | 2.3                          |
| 3            | 2019          | 97.7                       | 2.3                          |
| 4            | 2019          | 97.8                       | 2.2                          |
| 5            | 2019          | 97.5                       | 2.5                          |
| 6            | 2019          | 97.4                       | 2.6                          |
| 7            | 2019          | 97.4                       | 2.6                          |
| 8            | 2019          | 97.2                       | 2.8                          |
| 9            | 2019          | 97.1                       | 2.9                          |
| 3            | 2020          | 97.3                       | 2.7                          |
| 4            | 2020          | 97.0                       | 3.0                          |
| 5            | 2020          | 96.7                       | 3.3                          |
| 6            | 2020          | 96.8                       | 3.2                          |
| 7            | 2020          | 96.7                       | 3.3                          |
| 8            | 2020          | 96.5                       | 3.5                          |

Retail takes a significant share in the total sales but share is gradually decreasing.

### 7. What is the percentage of sales by demographic for each year in the dataset?
```sql
SELECT
  s.calendar_year,  demographic,  ROUND(100 *(SUM(sales) :: numeric / total_sales), 1) AS percentage
FROM
  clean_weekly_sales AS S
  JOIN (
    SELECT
      cSUMalendar_year,
      SUM(sales) AS total_sales
    FROM
      clean_weekly_sales AS T
    GROUP BY
      calendar_year
  ) T ON S.calendar_year = T.calendar_year
GROUP BY
  1, 2, total_sales
ORDER BY
  1, 2
```

| calendar_year | demographic | percentage  |
|---------------|-------------|-------------|
| 2018          | Couples     | 26.4        |
| 2018          | Families    | 32.0        |
| 2018          | unknown     | 41.6        |
| 2019          | Couples     | 27.3        |
| 2019          | Families    | 32.5        |
| 2019          | unknown     | 40.3        |
| 2020          | Couples     | 28.7        |
| 2020          | Families    | 32.7        |
| 2020          | unknown     | 38.6        |

### 8. Which age_band and demographic values contribute the most to Retail sales?
```sql
SELECT
  age_band, demographic,  SUM(sales) AS total_sales
FROM
  clean_weekly_sales
WHERE
  platform = 'Retail'
GROUP BY
  1,  2
ORDER BY
  3 DESC, 1, 2
```
We can see that most of age band and demographic characteristics of our users are unknown. Among the others, Families and Retirees contribute in retail sales the most.Now getting the maximum sales for age band and for demographic, excluding unknown values.

```sql
SELECT
  age_band, total_sales, percentage_of_sales
FROM
  (
    SELECT
      age_band,
      SUM(sales) AS total_sales,  round(100 * (SUM(sales) :: numeric / all_sales), 1) AS percentage_of_sales,
      row_number() over (
        ORDER BY
          SUM(sales) DESC
      )
    FROM
      clean_weekly_sales,
      LATERAL (
        SELECT
          SUM(sales) AS all_sales
        FROM
          clean_weekly_sales
      ) S
    WHERE
      platform = 'Retail'
      AND demographic != 'unknown'
    GROUP BY
      1, all_sales
  ) T
WHERE
  row_number = 1
```

| age_band | total_sales | percentage_of_sales  |
|----------|-------------|----------------------|
| Retirees | 13005266930 | 31.9                 |

```sql
SELECT
  demographic,  total_sales,  percentage_of_sales
FROM
  (
    SELECT
      demographic,  SUM(sales) AS total_sales,
      round(100 * (SUM(sales) :: numeric / all_sales), 1) AS percentage_of_sales,
      row_number() over (
        ORDER BY
          SUM(sales) DESC
      )
    FROM
      clean_weekly_sales,
      LATERAL (
        SELECT
          SUM(sales) AS all_sales
        FROM
          clean_weekly_sales
      ) S
    WHERE
      platform = 'Retail'
      AND demographic != 'unknown'
    GROUP BY
      1, all_sales
  ) T
WHERE
  row_number = 1
```

| demographic | total_sales | percentage_of_sales  |
|-------------|-------------|----------------------|
| Families    | 12759667763 | 31.3                 |

```sql
SELECT
  demographic,
  age_band,
  total_sales,
  percentage_of_sales
FROM
  (
    SELECT
      demographic, age_band,  SUM(sales) AS total_sales,
      round(100 * (SUM(sales) :: numeric / all_sales), 1) AS percentage_of_sales,
      row_number() over (
        ORDER BY
          SUM(sales) DESC
      )
    FROM
      clean_weekly_sales,
      LATERAL (
        SELECT
          SUM(sales) AS all_sales
        FROM
          clean_weekly_sales
      ) S
    WHERE
      platform = 'Retail'
      AND demographic != 'unknown'
    GROUP BY
      1, 2, all_sales
  ) T
WHERE
  row_number = 1
```

| demographic | age_band | total_sales | percentage_of_sales  |
|-------------|----------|-------------|----------------------|
| Families    | Retirees | 6634686916  | 16.3                 |

The age group of retirees and the demographic group of families and the combined group of retired families contribute the most to retail sales.

### 9. Can we use the avg_transaction column to find the average transaction size for each year for Retail vs Shopify? If not - how would you calculate it instead?

We can not use avg_transaction column to find the average transaction size per year and sales platform, because we need to aggregate it first. since we can not use the average of average to calculate the average.

```sql
SELECT
  calendar_year, platform, ROUND(SUM(sales) :: numeric / SUM(transactions), 1) AS correct_avg,
  ROUND(AVG(avg_transaction), 1) AS incorrect_avg
FROM
  clean_weekly_sales
GROUP BY
  1, 2
ORDER BY
  1, 2
```

| calendar_year | platform | correct_avg | incorrect_avg  |
|---------------|----------|-------------|----------------|
| 2018          | Retail   | 36.6        | 42.9           |
| 2018          | Shopify  | 192.5       | 188.3          |
| 2019          | Retail   | 36.8        | 42.0           |
| 2019          | Shopify  | 183.4       | 177.6          |
| 2020          | Retail   | 36.6        | 40.6           |
| 2020          | Shopify  | 179.0       | 174.9          |

## 3. Before & After Analysis

This technique is usually used when we inspect an important event and want to inspect the impact before and after a certain point in time.

Taking the week_date value of 2020-06-15 as the baseline week where the Data Mart sustainable packaging changes came into effect.

We would include all week_date values for 2020-06-15 as the start of the period after the change and the previous week_date values would be before

### 1. What is the total sales for the 4 weeks before and after 2020-06-15? What is the growth or reduction rate in actual values and percentage of sales?

```sql
WITH sales_before AS (
    SELECT
      SUM(sales) AS total_sales_before
    FROM
      clean_weekly_sales,
      LATERAL(
        SELECT
          EXTRACT(
            WEEK
            FROM
              '2020-06-15' :: date
          ) AS base_week
      ) B
    WHERE
      calendar_year = 2020
      AND week_number between (base_week - 4)
      AND (base_week - 1)
  ),
  sales_after AS (
    SELECT
      SUM(sales) AS total_sales_after
    FROM
      clean_weekly_sales,
      LATERAL(
        SELECT
          EXTRACT(
            WEEK
            FROM
              '2020-06-15' :: date
          ) AS base_week
      ) B
    WHERE
      calendar_year = 2020
      AND week_number between (base_week)
      AND (base_week + 3)
  )
SELECT
  total_sales_before, total_sales_after, total_sales_after - total_sales_before AS change_in_sales,
  ROUND(
    100 * (total_sales_after - total_sales_before) :: numeric / total_sales_before,
    2
  ) AS percentage_of_change
FROM
  sales_before,
  sales_after
```

| total_sales_before | total_sales_after | change_in_sales | percentage_of_change  |
|--------------------|-------------------|-----------------|-----------------------|
| 2345878357         | 2318994169        | -26884188       | -1.15                 |

The total sales for 4 weeks after week 25 decreased to 1.15% compared to 4 week sales before week 25

### 2. What about the entire 12 weeks before and after?

```sql
WITH sales_before AS (
    SELECT
      SUM(sales) AS total_sales_before
    FROM
      clean_weekly_sales,
      LATERAL(
        SELECT
          EXTRACT(
            WEEK
            FROM
              '2020-06-15' :: date
          ) AS base_week
      ) B
    WHERE
      calendar_year = 2020
      AND week_number between (base_week - 12)
      AND (base_week - 1)
  ),
  sales_after AS (
    SELECT
      SUM(sales) AS total_sales_after
    FROM
      clean_weekly_sales,
      LATERAL(
        SELECT
          EXTRACT(
            WEEK
            FROM
              '2020-06-15' :: date
          ) AS base_week
      ) B
    WHERE
      calendar_year = 2020 
      AND week_number between (base_week)
      AND (base_week + 11)
  )
SELECT
  total_sales_before,  total_sales_after,
  total_sales_after - total_sales_before AS change_in_sales,
  ROUND(
    100 * (total_sales_after - total_sales_before) :: numeric / total_sales_before,
    2
  ) AS percentage_of_change
FROM
  sales_before,  sales_after
```

| total_sales_before | total_sales_after | change_in_sales | percentage_of_change  |
|--------------------|-------------------|-----------------|-----------------------|
| 7126273147         | 6973947753        | -152325394      | -2.14                 |

The total sales for 12 weeks after week 25 decreased to 2.14% compared to 12 week sales before week 25

### 3. How do the sale metrics for these 2 periods before and after compare with the previous years in 2018 and 2019?

```sql
WITH sales_before AS (
    SELECT
      calendar_year,
      SUM(sales) AS total_sales_before
    FROM
      clean_weekly_sales,
      LATERAL(
        SELECT
          EXTRACT(
            WEEK
            FROM
              '2020-06-15' :: date
          ) AS base_week
      ) bw
    WHERE
      week_number between (base_week - 12)
      AND (base_week - 1)
    group by
      1
  ),
  sales_after AS (
    SELECT
      calendar_year,  SUM(sales) AS total_sales_after
    FROM
      clean_weekly_sales,
      LATERAL(
        SELECT
          EXTRACT(
            WEEK
            FROM
              '2020-06-15' :: date
          ) AS base_week
      ) B
    WHERE
      week_number between (base_week)
      AND (base_week + 11)
    group by
      1
  )
SELECT
  sb.calendar_year, total_sales_before,  total_sales_after,
  total_sales_after - total_sales_before AS change_in_sales,
  ROUND(
    100 * (total_sales_after - total_sales_before) :: numeric / total_sales_before,
    2
  ) AS percentage_of_change
FROM
  sales_before AS S
  JOIN sales_after AS B ON S.calendar_year = B.calendar_year
group by
  1, 2, 3
``` 
  
| calendar_year | total_sales_before | total_sales_after | change_in_sales | percentage_of_change  |
|---------------|--------------------|-------------------|-----------------|-----------------------|
| 2018          | 6396562317         | 6500818510        | 104256193       | 1.63                  |
| 2019          | 6883386397         | 6862646103        | -20740294       | -0.30                 |
| 2020          | 7126273147         | 6973947753        | -152325394      | -2.14                 |

We can see sales increased in 2018, and sales fell in 2019 and 2020 with 2020 having the worst performance.
