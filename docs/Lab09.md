# Lab10: Aggregate Data in Elasticsearch


You work as a data analyst for an online banking company that uses a 3-node Elasticsearch cluster as a NoSQL database for active accounts. You have been asked to determine the answers to a series of questions using the Elasticsearch search API. Your searches should not return any documents and should only provide the aggregation result.

1. How many unique employers are there among our account holders?
2. How many accounts do we have in each of the 50 US states?
3. What is the average balance for each of the 50 US states, and what state has the maximum average balance?

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.


### 1. Create an aggregation to answer question 1.

Use the Kibana console tool to execute the following:
```
GET bank/_search
{
  "size": 0,
  "aggs": {
    "employers": {
      "cardinality": {
        "field": "employer.keyword"
      }
    }
  }
}
```
### 2. Create an aggregation to answer question 2.

Use the Kibana console tool to execute the following:
```
GET bank/_search
{
  "size": 0,
  "aggs": {
    "state": {
      "terms": {
        "field": "state.keyword",
        "size": 100
      }
    }
  }
}
```
you can find the Italia state previously added.


### 3. Create an aggregation to answer question 3.

Use the Kibana console tool to execute the following:
```
GET bank/_search
{
  "size": 0,
  "aggs": {
    "state": {
      "terms": {
        "field": "state.keyword",
        "size": 50
      },
      "aggs": {
        "balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    },
    "max_average_balance": {
      "max_bucket": {
        "buckets_path": "state>balance"
      }
    }
  }
}
```
