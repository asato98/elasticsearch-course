# Lab10: Enable Elasticsearch Cluster Monitoring

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