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
