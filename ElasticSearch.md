# Elastic Search

## What is elastic search?

When people ask, “what is Elasticsearch?”, some may answer that it’s “an index”, “a search engine”, an “analytics database”, “a big data solution”, that “it’s fast and scalable”, or that “it’s kind of like Google”. Depending on your level of familiarity with this technology, these answers may either bring you closer to an ah-ha moment or further confuse you. But the truth is, all of these answers are correct and that’s part of the appeal of Elasticsearch. Over the years, Elasticsearch and the ecosystem of components that’s grown around it called the “Elastic Stack” has been used for a growing number of use cases, from simple search on a website or document, collecting and analyzing log data, to a business intelligence tool for data analysis and visualization.

- Elasticsearch is the distributed search and analytics engine at the heart of the Elastic Stack.
- Logstash and Beats facilitate collecting, aggregating, and enriching your data and storing it in Elasticsearch.
- Kibana enables you to interactively explore, visualize, and share insights into your data and manage and monitor the stack. Elasticsearch is where the indexing, search, and analysis magic happens.

While not *every* problem is a search problem, Elasticsearch offers speed and flexibility to handle data in a wide variety of use cases:

- Add a search box to an app or website
- Store and analyze logs, metrics, and security event data
- Use machine learning to automatically model the behavior of your data in real-time
- Automate business workflows using Elasticsearch as a storage engine
- Manage, integrate, and analyze spatial information using Elasticsearch as a geographic information system (GIS)
- Store and process genetic data using Elasticsearch as a bioinformatics research tool

# Components

To understand how elastic search works we need to lean about some concepts and components of this system.

## Logical Concepts

### Document

Documents are the basic unit of information that can be indexed in Elasticsearch expressed in JSON. You can think of a document like a row in a relational database, representing a given entity. It can be any structured data encoded in JSON.

### Indices

Indexing can help us to retrieve data with low latency.

An index is a collection of documents that have similar characteristics. An index is the highest level entity that you can query against in Elasticsearch. You can think of the index as being similar to a database in a relational database schema. Any documents in an index are typically logically related. In the context of an e-commerce website, for example, you can have an index for Customers, one for Products, one for Orders, and so on.

### Inverted Index

An index in Elasticsearch is actually what’s called an inverted index, which is the mechanism by which all search engines work. It is a data structure that stores a mapping from content, such as words or numbers, to its locations in a document or a set of documents. Basically, it is a hashmap-like data structure that directs you from a word to a document. For example, in the image below, the term “best” occurs in document 2, so it is mapped to that document.

![Untitled](ElasticSearch/Untitled.png)

Algorithm

- It tokenizes the content of each document.
- It creates a sorted list of all unique terms.
- It Associates the list of documents which have the word.

## Backend concepts

### Cluster

An Elasticsearch cluster is a group of one or more node instances that are connected together. The power of an Elasticsearch cluster lies in the distribution of tasks, searching, and indexing, across all the nodes in the cluster.

### Node

A node is a single server that is a part of a cluster. A node stores data and participates in the cluster’s indexing and search capabilities. An Elasticsearch node can be configured in different ways:

- **Master Node** — Controls the Elasticsearch cluster and is responsible for all cluster-wide operations like creating/deleting an index and adding/removing nodes.
- **Data Node** — Stores data and executes data-related operations such as search and aggregation.
- **Client Node** — Forwards cluster requests to the master node and data-related requests to data nodes.

### Shards

Elasticsearch provides the ability to subdivide the index into multiple pieces called shards. Each shard is in itself a fully-functional and independent “index” that can be hosted on any node within a cluster. By distributing the documents in an index across multiple shards, and distributing those shards across multiple nodes, Elasticsearch can ensure redundancy, which both protects against hardware failures and increases query capacity as nodes are added to a cluster.

### Replicas

Elasticsearch allows you to make one or more copies of your index’s shards which are called “replica shards” or just “replicas”. Basically, a replica shard is a copy of a primary shard. Each document in an index belongs to one primary shard. Replicas provide redundant copies of your data to protect against hardware failure and increase capacity to serve read requests like searching or retrieving a document.

# The Elastic Stack - ELK

Elasticsearch is the central component of the Elastic Stack, a set of open-source tools for data ingestion, enrichment, storage, analysis, and visualization.

- Elastic
- Logstash
- Kibana

## Kibana

[Kibana](https://www.knowi.com/blog/grafana-vs-kibana/) is a data visualization and management tool for Elasticsearch that provides real-time histograms, line graphs, pie charts, and maps. It lets you visualize your Elasticsearch data and navigate the Elastic Stack.

Kibana is often used for log analysis, it allows you to answer questions about where your web hits are coming from, your distribution URLs, and so on.

## Logstash

Logstash is used to aggregate and process data and send it to Elasticsearch. It is an open-source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to collect.

### Beats

Beats is a collection of lightweight, single-purpose data shipping agents used to send data from hundreds or thousands of machines and systems to Logstash or Elasticsearch. Beats are great for gathering data as they can sit on your servers, with your containers, or deploy as functions then centralize data in Elasticsearch. For example, Filebeat can sit on your server, monitor log files as they come in, parses them, and import into Elasticsearch in near-real-time.

## Primary use cases

### Application Search

For applications that rely heavily on a search platform for the accessing, retrieval, and reporting of data.

### Website Search

Websites which store a lot of content find elastic search a very useful tool for effective and accurate searches

### Enterprise Search

Elasticsearch allows enterprise-wide search that includes document search, E-commerce product search, blog search, people search, and any form of search you can think of. In fact, it has steadily penetrated and replaced the search solutions of most of the popular websites we use on a daily basis. From a more enterprise-specific perspective, Elasticsearch is used to great success in company intranets.

### Log analytics

Elasticsearch is commonly used for ingesting and analyzing log data in near-real-time and in a scalable manner. It also provides important operational insights on log metrics to drive actions.

### **Infrastructure metrics and container monitoring**

Many companies use the ELK stack to analyze various metrics. This may involve gathering data across several performance parameters that vary by use case.

### **Security analytics**

Access logs and similar logs concerning system security can be analyzed with the ELK stack, providing a more complete picture of what’s going on across your systems in real-time.

# Get Started

## Create new docker instance

```bash
docker network create somenetwork
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:tag
```

Then use the curl to make a new request

```bash
🕒 18:04 ⚡ ~ 👾 curl -X GET http://localhost:9200
{
  "name" : "c6faea14fccb",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "7lqeYSCFTyu1icvoPMmU2A",
  "version" : {
    "number" : "7.14.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "66b55ebfa59c92c15db3f69a335d500018b3331e",
    "build_date" : "2021-08-26T09:01:05.390870785Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

## Store Data

You add data to Elasticsearch as JSON objects called documents. Elasticsearch stores these documents in searchable indices.

For time series data, such as logs and metrics, you typically add documents to a data stream made up of multiple auto-generated backing indices.

### Add Document

## Retrieving

There are some definitions here

- TF, Term Frequency: Frequency of term in given document.
- DF, Document Frequency: Frequency of term in all documents.
- IDF, Inverse Document Frequency: $IDF = \frac{1}{DF}$
- Relevance: $Relevance= TF \times IDF$ or $Relevance = \frac{TF}{DF}$

By using this HTTP call you can retrieve a specific document.

```bash
curl -X GET http://localhost:9200/test-index/_doc/1 --user elastic:password | json_pp
{
   "_id" : "1",
   "_index" : "test-index",
   "_primary_term" : 3,
   "_seq_no" : 1,
   "_source" : {
      "author" : "kimchy",
      "text" : "Elasticsearch: cool. bonsai cool.",
      "timestamp" : "2022-02-23T17:14:52.878937"
   },
   "_type" : "_doc",
   "_version" : 2,
   "found" : true
}
```

# Snippets

To create Kibana and elastic search next to each other:

```yaml
version: '3'
services:

  elasticsearch:
    image: elasticsearch:7.16.3
    container_name: general_elastic
    environment:
      - node.name=node01
      - cluster.name=es-cluster-7
      - discovery.type=single-node
      - "ES_JAVA_OPTS=-Xms128m -Xmx128m"
      - xpack.security.enabled=true
      - xpack.security.audit.enabled=true
      - ELASTIC_PASSWORD=password
    volumes:
      - es-data01:/usr/share/elasticsearch/data
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:
      - "9200:9200"
      - "9300:9300"

  kibana:
    image: kibana:7.16.2
    container_name: general_kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
      - ELASTICSEARCH_USERNAME=elastic
      - ELASTICSEARCH_PASSWORD=password
    depends_on: 
      - elasticsearch
    ports:
      - "5601:5601"

volumes:
  es-data01:
    driver: local
```

# Python SDK

```bash
from datetime import datetime
from elasticsearch import Elasticsearch

es = Elasticsearch('http://localhost:9200', http_auth=("elastic", "password"))

# Create a new document
doc = {
    'author': 'kimchy',
    'text': 'Elasticsearch: cool. bonsai cool.',
    'timestamp': datetime.now(),
}

# Add the document
resp = es.index(index="test-index", id=1, document=doc)
print(resp['result'])

# Retrieve the document
resp = es.get(index="test-index", id=1)
print(resp['_source'])

# Refresh the indecies
es.indices.refresh(index="test-index")

# Search between documents
resp = es.search(index="test-index", query={"match_all": {}})
print("Got %d Hits:" % resp['hits']['total']['value'])
for hit in resp['hits']['hits']:
    print("%(timestamp)s %(author)s: %(text)s" % hit["_source"])
```

## To get a specific item 

```python
fetched = es.get(index="ride-recommender-2022.06.29", id="Cp3xsIEBm0LuWQhlK45T")
```

# References

- [What is Elasticsearch? | Elasticsearch Guide [7.14] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/elasticsearch-intro.html)
- [Elasticsearch: What it is, How it works, and what it's used for](https://www.knowi.com/blog/what-is-elastic-search/)
- [Quick start | Elasticsearch Guide [7.16] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html)
- [How indexing and retrieval algorithms work in Elasticsearch | ElasticSearch 7 for Beginners #1.2](https://www.youtube.com/watch?v=fcIzAg63WyI)