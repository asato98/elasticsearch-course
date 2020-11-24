# Lab11: Diagnose and Repair Elasticsearch Indices

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