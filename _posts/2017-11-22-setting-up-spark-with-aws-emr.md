---
layout: post
title: Setting Up Spark with AWS EMR
comments: true
subdir: aws_emr
tags:
- Big Data
---

In this post, I will be setting up the Zepplin Notebook using AWS EMR(Elastic MapReduce). It is almost like the Jupyter Notebook but was created specifically for Big Data(Spark, Hadoop, etc). The current version is 0.7 and supports multiple languages.

## Set-up

AWS EMR is not a free service and the price varies depending on the instance types you pick. I proceeded without an EC2 key pair just for demo purpose.

{% include image.html subdir=page.subdir name='aws_emr_1.png' caption='Picture 1: Creating a AWS EMR cluster with spark' %}

{% include image.html subdir=page.subdir name='aws_emr_2.png' caption='Picture 2: Cluster is running' %}

I added security rules of SSH and Custom TCP Rule(port 8890) on EMR-master so that I can only access to it. Under summary of the cluster, you get a master public DNS and let's copy and paste it into the browser. It's time to start our first Notebook!

## Zepplin Notebook

The first thing is adding "%pyspark" in the cell so that everything else in the cell can be in Python. Otherwise, it expects Scala.

Luckly, the Zepplin Notebook creates a sequel context ready(sc) and it can deal with data in AWS S3 bucket.

{% include image.html subdir=page.subdir name='aws_emr_6.png' caption='Picture 3: Using pyspark in the Notebook' %}

