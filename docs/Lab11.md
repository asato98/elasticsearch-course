# Lab12: Use Metric beats




### 1. Install metricbeat.
let's start with updating repositories

```
apt-get update
```
then install metricbeat:
```
apt-get install metricbeat=7.6.0
```
### 2. Configure Metricbeat to connect.

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
