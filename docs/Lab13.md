# Lab05: Working with Logstash and filebeat

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

Modify /etc/filebeat/filebeat.yml to set the connection information:
```
output.elasticsearch:
  hosts: ["<master_IP>:9200"]
  username: "elastic"
  password: "elastic_566"
setup.kibana:
  host: "<<master_IP>:8080"
```

For both the syslog and auth sections.

2.4 Enable the system and logstash Filebeat modules:
```
 filebeat modules enable system
 filebeat modules enable logstash
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
