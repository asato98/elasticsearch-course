# Lab10: Enable Elasticsearch Cluster Monitoring


You work as a data infrastructure engineer for a large IT shop where one of your DevOps teams want to utilize Elasticsearch for a new project. You have a 3-node Elasticsearch cluster with Kibana already created to use as a proof of concept for the project but you want to enable self-monitoring on the cluster so that you can observe the cluster performance in order to make informed design decisions for a later production deployment.

For this, you will need to enable the collection of monitoring data on the 3-node cluster. Since this is not a production cluster, it will be self-monitored so there will be no need to setup remote monitoring at this time.

Your master-1 node has a Kibana instance which can be accessed in your local web browser by navigating to the public IP address of the master-1 node over port 8080 (example: http://public_ip:8080). To log in, use the user: **elastic**  with the password: **elastic_566**.


### 1. Enable the collection of monitoring data.

Use the Kibana console tool to execute the following:
```
PUT _cluster/settings
{
  "persistent": {
    "xpack.monitoring.collection.enabled": true
  }
}
```
### 2. Explore the Monitoring data in Kibana.

From Kibana, navigate to the "Stack Monitoring" application and explore the monitoring data.