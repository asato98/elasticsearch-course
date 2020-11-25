# Lab11: Diagnose and Repair Elasticsearch Indices


You are the new administrator for a 3-node Elasticsearch cluster. Upon taking ownership of the cluster, you find that several indexes which are not in a green state. You are being asked by your superiors to prioritize the troubleshooting of the index allocation issues above all other work. All indices will need to be allocated with a green state and any indices that were previously red should be configured to fail to a yellow state should the same failures happen again.

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.


### 1. Troubleshoot any red-state indices.

Using the Secure Shell (SSH), log in to the data-1node ascloud_user` via the public IP address.

Start the elasticsearch node with:
```
sudo systemctl start elasticsearch
```
Use the Kibana console tool to execute the following:
```
PUT logs-02/_settings
{
  "number_of_replicas": 1
}
```
### 2. Troubleshoot any yellow-state indices.

Use the Kibana console tool to execute the following:
```
PUT logs-01/_settings
{
  "number_of_replicas": 1
}
```