# Spark

## Introduction

Apache Spark is a unified analytics engine for large-scale data processing.

Unlike most other shells however which let you manipulate data using disk and memory in one single machine, Sparks allows you to interact with data distributed on disk or memory in many machines available in the cluster.

### RDD

Resilient Distributed Datasets are distributed collections that are automatically parallelized across the cluster.

### Simple code in `PySpark`

PySpark is a python shell for spark. Here is a simple codes to start:

```python
>>> lines = sc.textFile("README.md") # Create an RDD called lines
>>> lines.count() # Count the number of items in this RDD
127
>>> lines.first() # First item in this RDD, i.e. first line of README.md
u'# Apache Spark'
```

You may find the logging statements that get printed in the shell distracting. You can control the verbosity of the logging. To do this, you can create a file in the conf directory called `log4j.properties`. The Spark developers already include a template for this file called `log4j.properties.template`. To make the logging less verbose, make a copy of `conf/log4j.properties.template` called `conf/log4j.properties` and find the following line:

```yaml
log4j.rootCategory=INFO, console
```

Then lower the log level so that we show only the WARN messages, and above by changing it to the following:

```yaml
log4j.rootCategory=WARN, console
```

# Spark components

At a high level, every Spark application consists of a `driver` program that launches various parallel operations on a cluster. When you are using the shell tool, actually the driver is the shell itself. The driver uses `SparkContext` (`sc` in code) to build RDDs and its other features.

- Contains the main function of your program
- Define Distributed Datasets `sc.textFile()`
- Operates actions `count`

To run these operations, driver programs typically manage a number of nodes called executors.

![Untitled](Spark/Untitled.png)

Another function is `filter`:

```python
>>> lines = sc.textFile("README.md")
>>> pythonLines = lines.filter(lambda line: "Python" in line)
>>> pythonLines.first()
u'## Interactive Python Shell'
```

function-based operations like filters also parallelize across the cluster. That is, Spark automatically takes your function (e.g., `line.contains("Python")`) and ships it to executor nodes. Thus, you can write code in a single driver program and automatically have parts of it run on multiple nodes.

## Standalone Application

The main difference from using it in the shell is that you need to initialize your own SparkContext. After that, the API is the same.

Here is the code for initialization in python:

```python
from pyspark import SparkConf, SparkContext

conf = SparkConf().setMaster("local").setAppName("My App")
sc = SparkContext(conf = conf)

# Read a text file and process with spark
lines = sc.textFile("README.md")
lines_len = lines.map(lambda l: len(l))
print("lines len:", lines_len.collect())
total_len = lines_len.reduce(lambda a, b: a + b)
print("total:", total_len)
```

- A cluster URL, namely local in these examples, tells Spark how to connect
to a cluster. local is a special value that runs Spark on one thread on the local
machine, without connecting to a cluster.
- An application name, namely My App in these examples. This will identify your
application on the cluster manager’s UI if you connect to a cluster.

## PySpark
It's really important to care about which version of `pyspark` you are using.

```python
import pyspark

print(pyspark.__version__)
```

### Sub Modules

There are some sub modules in pyspark
- Structured Data -> `pyspark.sql`
- Streaming Data -> `pyspark.streaming`
- Machine Learning -> `pyspark.mllib`

### Connection

For Remote cluster, You should specify the master node: `spark://<IP_ADDRESS|DNS_NAME>:<PORT>`

For local cluster, You can use `local` and the number of cores: `local`, `local[2]`, or `local[*]`

## Resources

[Docker Hub](https://hub.docker.com/r/bitnami/spark)

[GitHub - apache/spark: Apache Spark - A unified analytics engine for large-scale data processing](https://github.com/apache/spark)

[Youtube video](https://www.youtube.com/watch?v=czd9a2Rc-h4)

### Books

- O'Reilly Learning Spark - `Holden Karau`