# Elastic Search

```bash
from datetime import datetime
from elasticsearch import Elasticsearch

es = Elasticsearch('http://localhost:9200', basic_auth=("elastic", "password"))

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

To get a specific item

```python
fetched = es.get(index="ride-recommender-2022.06.29", id="Cp3xsIEBm0LuWQhlK45T")
```