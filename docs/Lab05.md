# Lab05: Working with Logstash

### 1. Install Logstash

Install Logstash with default settings:

1.1 Import the Logstash key:
```
 rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```
1.2 Add the Logstash repo:
```
 vi /etc/yum.repos.d/logstash.repo

 [logstash-6.x]
 name=Elastic repository for 6.x packages
 baseurl=https://artifacts.elastic.co/packages/6.x/yum
 gpgcheck=1
 gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
 enabled=1
 autorefresh=1
 type=rpm-md
 ```
1.3 Install Logstash
```
 yum install logstash -y
```
1.4 Enable and start Logstash:
```
 systemctl enable logstash
 systemctl start logstash
 ```
 ### 2. Install Filebeat and use the System Module

Install Filebeat with default settings and use the system module:

2.1 Download Filebeat:
```
 curl -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.2.3-x86_64.rpm
```
2.2 Install Filebeat:
```
 rpm --install filebeat-6.2.3-x86_64.rpm
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