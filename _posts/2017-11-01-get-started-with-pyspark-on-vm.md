---
layout: post
title: Get Started with PySpark on VM
comments: true
subdir: vm_spark
tags:
- Big Data
---

## Quick Note
- Spark 2.1 is incompatible with Python 3.6
- This will be resolved when Spark 2.2 is released
- Python 3.5 will be used in this post to avoid issues

## Setting Up
- [VirtualBox 5.2.2](https://www.virtualbox.org/wiki/Downloads)
- [Ubuntu 16.04.3 LTS](https://www.ubuntu.com/download/desktop)

{% include image.html subdir=page.subdir name='vm_1.png' caption='Picture 1: Install Oracle VM VirtualBox Manager' %}

Open a terminal and follow the commands.

```bash
$ sudo apt-get update

$ sudo apt install python3-pip
$ pip3 install jupyter

# automatically opens Jupyter Notebook in browser
# copy/paste a given URL from terminal into the browser at first time
$ jupyter notebook

# install java and scala
$ sudo apt-get install default-jre
$ sudo apt-get install scala

$ pip3 install py4j
```

Now, download [Apache Spark](spark.apache.org/downloads.html) and move it to the home folder.

{% include image.html subdir=page.subdir name='vm_4.png' caption='Picture 2: Downloading Apache Spark with 2.1.0 version' %}

```bash
# unzip the file
$ sudo tar -zxvf spark…tgz

# tell python to find spark
$ export SPARK_HOME=‘home/ubuntu/spark-2.1.0-bin-hadoop2.7’
$ export PATH=$SPARK_HOME:$PATH
$ export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
$ export PYSPARK_DRIVER_PYTHON=“jupyter”
$ export PYSPARK_DRIVER_PYTHON_OPTS=“notebook”
$ export PYSPARK_PYTHON=python3

# fix any possible permission errors when using Jupyter notebook under the spark folder 
$ sudo chmod 777 spark-2.1.0-bin-hadoop2.7
$ sudo chmod 777 spark-2.1.0-bin-hadoop2.7/python
$ sudo chmod 777 spark-2.1.0-bin-hadoop2.7/python/pyspark
```

Finally, let's open the Jupyter Notebook!

```bash
$ jupyter notebook
```

{% include image.html subdir=page.subdir name='vm_5.png' caption='Picture 3: Importing PySpark in the Jupyter Notebook' %}

However, there is one problem that PySpark works only if we are in the correct Spark directory folder. This can be fixed by using **findspark**.

```bash
# install findspark
$ pip3 install findspark

# now it works from any directory and we can apply this in the Notebook
$ python3
>>> import findspark
>>> findspark.init('/home/david/spark-2.1.0-bin-hadoop2.7')
>>> import pyspark
>>> quit()
```
