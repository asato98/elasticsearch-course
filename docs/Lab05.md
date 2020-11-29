# Lab05: Working with Logstash

### 1. Install Logstash
 
Install Logstash with default settings:
 
1.1 update repositories indexes:
```
 apt-get update
```
1.3 Install Logstash
```
 apt-get install logstash
```
1.4 Enable and start Logstash:
```
 systemctl enable logstash
 systemctl start logstash
```


### 2. Install Filebeat and use the System Module

Install Filebeat with default settings and use the system module:

2.2 Install Filebeat:
```
apt-get install filebeat=7.6.0
```
2.3 Edit the system module to convert timestamp timezones to UTC:

In /etc/filebeat/modules.d/system.yml.disabled, change:
```
 # Convert the timestamp to UTC. Requires Elasticsearch >= 6.1.
 #var.convert_timezone: false
```
To:
```
 # Convert the timestamp to UTC. Requires Elasticsearch >= 6.1.
 var.convert_timezone: true
```
For both the syslog and auth sections.

2.4 Enable the system Filebeat module:
```
 filebeat modules enable system
```
2.5 Install the ingest-geoip filter plugin for Elasticsearch ingest node:
```
 /usr/share/elasticsearch/bin/elasticsearch-plugin install ingest-geoip
```
2.6 Restart Elasticsearch so it can use the new ingest-geoip plugin:
```
 systemctl restart elasticsearch
```
2.7 Once Elasticsearch starts up, push module assets to Elasticsearch and Kibana:
```
 filebeat setup
```
2.8 Enable and start Filebeat:
```
 systemctl enable filebeat
 systemctl start filebeat
```
