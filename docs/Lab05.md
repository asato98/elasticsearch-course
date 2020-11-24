# Lab05: Define User Access Control in Elasticsearch 

### 1. Create the monitor role.
Use the Kibana console tool to execute the following:
```
POST _security/role/monitor
{
  "indices": [
    {
      "names": [
        "*"
      ],
      "privileges": [
        "read",
        "monitor"
      ]
    }
  ]
}
```
### 2. Create the noc user.

Use the Kibana console tool to execute the following:
```
POST _security/user/noc
{
  "roles": [
    "kibana_user",
    "monitor",
    "monitoring_user"
  ],
  "full_name": "Network Operations Center",
  "email": "noc@company.com",
  "password": "noc_566"
}
```
