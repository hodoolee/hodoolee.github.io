---
layout: post
title: Spark DataFrame Basics
comments: true
subdir: spark_df
tags:
- Big Data
---

## Spark DataFrame Basics

Spark DataFrames hold data in a column/row format. Each column represents feature or variable and each row represents an individual data point. Originally, Spark had a syntax called RDD but had shifted towards a DataFrame syntax which is much easier and cleaner to use.

We can input/ouput data from variety of sources and use DataFrames to apply transformations on the data so that we show or collect the results to display the final result. Let's start by importing a simple json file(people.json).

```json
people.json

{ "name": "Michael" }
{ "name": "Andy",   "age":30 }
{ "name": "Justin", "age":19 }
```

![spark_df_1]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_1.png)
![spark_df_2]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_2.png)

Notice that there are two columns and each column has long and string type. If you have issues with the file format, we can actually clarify new schema type.

![spark_df_3]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_3.png)


Let's try with more methods.

![spark_df_4]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_4.png)

You can use pure sequel directly to deal with the DataFrame.

![spark_df_5]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_5.png)

## Basic Operations

![spark_df_6]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_6.png)
![spark_df_7]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_7.png)
![spark_df_8]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_8.png)

## Groupby and Aggregate Operations

![spark_df_aggs_1]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_aggs_1.png)
![spark_df_aggs_2]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_aggs_2.png)
![spark_df_aggs_3]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_aggs_3.png)
![spark_df_aggs_4]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_aggs_4.png)
![spark_df_aggs_5]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_aggs_5.png)

## Missing Data

![spark_df_miss_1]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_miss_1.png)
![spark_df_miss_2]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_miss_2.png)
![spark_df_miss_3]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_miss_3.png)
![spark_df_miss_4]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_miss_4.png)

## Dates and Timestamps

![spark_df_dates_1]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_dates_1.png)
![spark_df_dates_2]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_dates_2.png)