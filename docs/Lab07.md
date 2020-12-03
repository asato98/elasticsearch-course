# Lab08: Execute CRUD Operations in Elasticsearch


You work as an Elasticsearch administrator for a banking company. A recent failed deployment and subsequent rollback of your banking software has desynchronized some actions that were taken against a few accounts. To quickly rectify the desynchronization, you are being asked to perform manual CRUD operations to the bank index in Elasticsearch using the Kibana console tool.

An account needs to be added with the following customer data:

Account Number: 1000
Balance: $65,536
Firstname: John
Lastname: Doe
Age: 23
Gender: Male
Address: 45 West 27th Street
Employer: Elastic
Email: john@elastic.com
City: New York
State: NY
Account 100 has changed addresses and needs the following fields updated:

Address: 1600 Pennsylvania Ave NW
City: Washington
State: DC
Accounts 1 and 10 have been closed by their previous owners and need to be deleted.

**NOTE: The document IDs and account numbers of each document in the bank index are the same.**

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.

### 1. Search data:

```
GET bank/_doc/4
GET bank/_doc/2
GET bank/_doc/100
```
verify that the document with id 1000 doesn’t exist:

```
GET bank/_doc/1000
```
```
{
  "_index" : "bank",
  "_type" : "_doc",
  "_id" : "1000",
  "found" : false
}
```


### 2. Create account 1000.

Use the Kibana console tool to execute the following:
```
PUT bank/_doc/1000
{
  "account_number": 1000,
  "balance": 65536,
  "firstname": "Marco",
  "lastname": "Benvenuti",
  "age": 23,
  "gender": "M",
  "address": "Via Galilei 6",
  "employer": "Elastic",
  "email": "M.Benvenuti@elastic.com",
  "city": "Roma",
  "state": "Italia"
}
```

check it again
```
GET bank/_doc/1000
```

### 3. Update the address for account 100.

check the account 100:
```
GET bank/_doc/100
```

Use the Kibana console tool to execute the following:
```
POST bank/_update/100/
{
  "doc": {
    "address": "via Alighieri",
    "city": "Milano",
    "state": "Italia"
  }
}
```
check it again

```
GET bank/_doc/100
```

### 3. Delete accounts 1 and 10.

Use the Kibana console tool to execute the following:
```
DELETE bank/_doc/1
DELETE bank/_doc/10
```
