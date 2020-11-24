# Lab 01: Deploy a Multi-Node Elasticsearch Cluster

### 1. Install Elasticsearch on each node.

Using the Secure Shell (SSH), log in to each node as cloud_user via the public IP address.

Become the root user with:
```
sudo su -
```
Import the Elastic GPG key:
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```
Download the Elasticsearch 7.6 RPM:
```
curl -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.0-x86_64.rpm
```
Install Elasticsearch:
```
rpm --install elasticsearch-7.6.0-x86_64.rpm
```
Configure Elasticsearch to start on system boot:
```
systemctl enable elasticsearch
```
### 2. Configure each node's elasticsearch.yml per instructions.

Log in to each node and become the root user:
```
sudo su -
```
Open the elasticsearch.yml file:
```
vim /etc/elasticsearch/elasticsearch.yml
```
Change the following line:
```
#cluster.name: my-application
```
to
```
cluster.name: cluster-1
```
Change the following line on master-1:
```
#node.name: node-1
```
to
```
node.name: master-1
```
Change the following line on data-1:
```
#node.name: node-1
```
to
```
node.name: data-1
```
Change the following line on data-2:
```
#node.name: node-1
```
to
```
node.name: data-2
```
Change the following line on data-1:
```
#node.attr.rack: r1
```
to
```
node.attr.temp: hot
```
Change the following line on data-2:
```
#node.attr.rack: r1
```
to
```
node.attr.temp: warm
```
Add the following lines on master-1:
```
node.master: true
node.data: false
node.ingest: false
node.ml: false
```
Add the following lines on data-1:
```
node.master: false
node.data: true
node.ingest: true
node.ml: false
```
Add the following lines on data-2:
```
node.master: false
node.data: true
node.ingest: true
node.ml: false
```
Change the following on each node:
```
#network.host: 192.168.0.1
```
to
```
network.host: [_local_, _site_]
```
Change the following on each node:
```
#discovery.seed_hosts: ["host1", "host2"]
```
to
```
discovery.seed_hosts: ["10.0.1.101"]
```
Change the following on each node:
```
#cluster.initial_master_nodes: ["node-1", "node-2"]
```
to
```
cluster.initial_master_nodes: ["master-1"]
```
### 3. Configure the heap for each node per instructions.

Log in to the master node and become the root user:
```
sudo su -
```
Open the jvm.options file:
```
vim /etc/elasticsearch/jvm.options
```
Change the following lines:
```
-Xms1g
-Xmx1g
```
to
```
-Xms768m
-Xmx768m
```
Log in to each data node and become the root user:
```
sudo su -
```
Open the jvm.options file:
```
vim /etc/elasticsearch/jvm.options
```
Change the following lines:
```
-Xms1g
-Xmx1g
```
to
```
-Xms2g
-Xmx2g
```

### 4. Start Elasticsearch on each node.

Log in to each node and become the root user:
```
sudo su -
```
Start Elasticsearch:
```
systemctl start elasticsearch
```
Check the startup process:
```
less /var/log/elasticsearch/cluster-1.log
```
Check the node configuration:
```
curl localhost:9200/_cat/nodes?v
```

