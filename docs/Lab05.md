# Lab06: Define User Access Control in Elasticsearch 


You are the system administrator for a 3-node Elasticsearch cluster. In order to better support your cluster, you will have your Network Operations Center (NOC) handle all of the day-to-day monitoring of your cluster and its data so that it can quickly identify and report any percieved issues. For this, you will need to give the NOC access to the cluster but in order to follow security best practices, you will need to give them the least amount of permissions possible to do their job.

You will need to create a new role called monitor that has read and monitor permissions to all indexes in your cluster. Then, you will need to create a new user called noc who will be given the custom monitor role in addition to the built in roles kibana_user and monitoriing_user which are required for access to Kibana's Monitoring plugin. Beyond that, the noc user should have no further permissions to the cluster. The noc user should be created as follows:

Username: noc
Full Name: Network Operations Center
Email: noc@company.com
Roles: monitor, kibana_user, monitoring_user
Password: noc_566

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.


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
