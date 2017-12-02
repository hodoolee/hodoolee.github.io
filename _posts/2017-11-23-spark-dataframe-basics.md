---
layout: post
title: Spark DataFrame Basics
comments: true
subdir: spark_df
tags:
- Big Data
---

Spark DataFrames hold data in a clumn and row format. Each column represents feature or variable and each row represents an individual data point. Originally, Spark had a syntax called RDD but had shifted towards a DataFrame syntax which is much easier and cleaner to use.

We can input/ouput data from variety of sources and use these DataFrames to apply transformations on the data so that we show or collect the results to display the final result. In this post, I will import a simple json file(people.json) and do some Spark DataFrame methods.

![spark_df_1]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_1.png)

![spark_df_2]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_2.png)

Notice that there are two columns and each column has long and string types. Using type tools, we can also define a new schema.

![spark_df_3]({{ site.url }}/assets/images/{{ page.subdir }}/spark_df_3.png)

So if you ever have issues with the file formats, use this methodology to actually clarify the schema type.


