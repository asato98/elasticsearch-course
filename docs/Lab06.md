# Lab06: Define Elasticsearch Indices 

### 1. Create the logs-01 index.

Use the Kibana console tool to execute the following:
```
PUT /logs-01
{
  "aliases": {
    "this_week": {}
  },
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
```
### 2. Create the logs-02 index.

Use the Kibana console tool to execute the following:
```
PUT /logs-02
{
  "aliases": {
    "last_week": {}
  },
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
```
