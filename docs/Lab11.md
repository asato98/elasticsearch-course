# Lab12: Use Metric beats

### 1. Install Filebeat and use the System Module

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

Check the syslog logs on Kibana:

```
<IP_master:8080>
```


### 2. Install metricbeat.
let's start with updating repositories

```
apt-get update
```
then install metricbeat:
```
apt-get install metricbeat=7.6.0
```
### 3. Configure Metricbeat to connect.

move into configuration path
```
cd /etc/metricbeat/
```
and edit metricbeat.yml file with your cluster values

```
output.elasticsearch:
  hosts: ["<master_IP>:9200"]
  username: "elastic"
  password: "elastic_566"
setup.kibana:
  host: "<<master_IP>:8080"
```
Now enable system module and setup

```
metricbeat modules enable system
metricbeat setup
```

at the end of configuration enable and start the service
```
systemctl enable metricbeat
systemctl start metricbeat
```

### 3. Repeat the step 1. and 2. on data1 and data2 nodes
