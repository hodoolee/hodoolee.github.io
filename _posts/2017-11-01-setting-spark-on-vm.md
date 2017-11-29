---
layout: post
title: Setting Spark on VirtualMachine
comments: true
subdir: vm
tags:
- Big Data
---

## Note
- Spark 2.1 is incompatible with Python 3.6
- This will be resolved when Spark 2.2 is released
- Python 3.5 will be used in this post to avoid issues


![vm_1]({{ site.url }}/assets/images/{{ page.subdir }}/vm_1.png)

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

![vm_3]({{ site.url }}/assets/images/{{ page.subdir }}/vm_3.png)


Download Spark file and move it to home folder.

![vm_4]({{ site.url }}/assets/images/{{ page.subdir }}/vm_4.png)

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

$ python3
>>> import pyspark
>>> quit()
```

```bash
$ jupyter notebook
```

In the notebook, let's import pyspark!

![vm_5]({{ site.url }}/assets/images/{{ page.subdir }}/vm_5.png)

{% include image.html subdir=page.subdir name='vm_5.png' caption='' %}
