# Lab06: Define Elasticsearch Indices 


You work as a system administrator for a 3-node Elasticsearch cluster. You are being asked to prepare the Elasticsearch cluster to ingest some log data by creating the neccessary indices. We would like to search the data by using aliases such as this_week or last_week. This makes it easy to search the data you care about since you don't have to know the specific index names. Using the console tool in Kibana, create the following indices:
``` 
+---------+-----------+----------------+----------------+
| Name    | Alias     | Primary Shards | Replica Shards |
+---------+-----------+----------------+----------------+
| logs-01 | this_week | 2              | 1              |
+---------+-----------+----------------+----------------+
| logs-02 | last_week | 2              | 1              |
+---------+-----------+----------------+----------------+
```

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.


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
### 3. Delete index.

Use the Kibana console tool to execute the following:

```
DELETE logs-01
DELETE logs-02
```
Result in:
```
{
  "acknowledged" : true
}
```

### 4. Add 3 new indeces.

create the first index "bank"
```
PUT bank
{
  "settings": {
    "number_of_shards": 1
    , "number_of_replicas": 1
  }
}
```
create the second index "sheakspeare"
```
PUT shakespeare
{
  "mappings": {
    "properties": {
      "speaker": {
        "type": "keyword"
      },
      "play_name": {
        "type": "keyword"
      },
      "line_id": {
        "type": "integer"
      },
      "speech_number": {
        "type": "integer"
      }
    }
  },
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}
```
create the second index "sheakspeare"
```
PUT logs
{
  "mappings": {
    "properties": {
      "geo": {
        "properties": {
          "coordinates": {
            "type": "geo_point"
          }
        }
      }
    }
  },
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}
```


### 5. Bulk Index Data.
Now let’s go to add datato our indices with BULK API operation:

on Master node download the files that contains data:
```
mkdir bulk-data
cd bulk-data
```
```
curl -O https://raw.githubusercontent.com/nabilsato/elasticstack/main/accounts.json
```
```
curl -O https://raw.githubusercontent.com/nabilsato/elasticstack/main/shakespeare.json
```
```
curl -O https://raw.githubusercontent.com/nabilsato/elasticstack/main/logs.json
```
check the files are downloaded:

```
ls
```
```
root@01-master:~/bulk-data# ls
accounts.json  logs.json  shakespeare.json
```
check the content of the files:

```
head accounts.json
```
Let's Bulk data from JSON files to the appropriate index:
```
curl -u elastic -k -H 'Content-type: application/x-ndjson' -X POST http://localhost:9200/bank/_bulk --data-binary @accounts.json
```
```
curl -u elastic -k -H 'Content-type: application/x-ndjson' -X POST http://localhost:9200/bank/_bulk --data-binary @shakespeare.json
```
```
curl -u elastic -k -H 'Content-type: application/x-ndjson' -X POST http://localhost:9200/bank/_bulk --data-binary @logs.json
```
and at the end check the data on Kibana (dev tool):

```
GET _cat/nodes?v
```
```
GET _cat/indices?v
```
refresh the indices:
```
POST /bank/_refresh

POST /shakespeare/_refresh

POST /logs/_refresh
```
the documents are added and you will see the shakespeare index like this:
```
green  open   shakespeare                      GhpXu5ziR7Gh93J6gbEYkg   1   1     111396            0       39mb         19.5mb
```

