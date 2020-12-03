# Lab08: Search Data in Elasticsearch


You work as an Elasticsearch consultant and have been hired by a local university looking to implement Elasticsearch for literary research. The team you are working with is creating a UI that will enable students to perform search analysis on various works of literature. The test setup you are working with is a 3-node Elasticsearch cluster loaded with the complete works of Shakespeare. In order for the UI to display the desired search results, you must help the team come up with a few search requests that meet the following requirements:

Query 1:

A term-level search where the State is "CO" .

Query 2:

A term-level search that returns the first 25 results where the gender is "M".

Query 3:

A full-text search that returns the first 5 results where the text entry contains the word "London".

Query 4:

A full-text search where the text entry contains the phrase "O Romeo".

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.



### 1. Create a search query that meets the requirements of Query 1.

Use the Kibana console tool to execute the following:

for global search
```
GET bank/_search
```
and you can define many hit you need to show:
```
GET bank/_search?size=3

GET bank/_search?size=50
```
now search with a query

```
GET bank/_search
{
  "size": 1000, 
  "query": {
    "terms": {
      "state.keyword": [
        "CO"
      ]
    }
  }
}
```
### 2. Create a search query that meets the requirements of Query 2.

Use the Kibana console tool to execute the following:
```
GET bank/_search
{
  "size": 25, 
  "query": {
    "terms": {
      "gender.keyword": [
        "M"
      ]
    }
  }
}
```
### 3. Create a search query that meets the requirements of Query 3.

Use the Kibana console tool to execute the following:
```
GET shakespeare/_search
{
  "size": 5, 
  "query": {
    "match": {
      "text_entry": "London"
    }
  }
}
```
### 4. Create a search query that meets the requirements of Query 4.

Use the Kibana console tool to execute the following:
```
GET shakespeare/_search
{
  "query": {
    "match_phrase": {
      "text_entry": "O Romeo"
    }
  }
}
```
