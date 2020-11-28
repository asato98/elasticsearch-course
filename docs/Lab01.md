# Lab 01: Deploy a Multi-Node Elasticsearch Cluster


You are a system administrator who has been asked to deploy a 3-node Elasticsearch cluster with very specific configuration requirements:

You will need to install Elasticsearch version 7.6.0 from an RPM at https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.0-x86_64.rpm.
Each Elasticsearch instance will need to listen on both the local and site-local addresses.
Configure each node of the cluster-1 cluster as outlined in the table below.
```
+----------+-----------+------------+--------------+----------+
| Server   | Node Name | Attributes | Roles        | JVM Heap |
+----------+-----------+------------+--------------+----------+
| master-1 | master-1  |            | master       | 768m     |
+----------+-----------+------------+--------------+----------+
| data-1   | data-1    | temp=hot   | data, ingest | 2g       |
+----------+-----------+------------+--------------+----------+
| data-2   | data-2    | temp=warm  | data, ingest | 2g       |
+----------+-----------+------------+--------------+----------+
```

### 1. Install Elasticsearch on each node.

Using the Secure Shell (SSH), log in to each node as cloud_user via the public IP address.

Become the root user with:
```
sudo su -
```
Import the Elastic GPG key:
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
Add Elastic repository:
```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```
Install Elasticsearch:
```
apt-get update && sudo apt-get install elasticsearch=7.6.0
```
Configure Elasticsearch to start on system boot:
```
sudo systemctl daemon-reload
```
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
cluster.name: cluster-<n_studente>
```
Change the following line on *<n_studente>*-master-1:
```
#node.name: node-1
```
to
```
node.name: <n_studente>-master
```
Change the following line on *<n_studente>*-data-1:
```
#node.name: node-1
```
to
```
node.name: <n_studente>-data1
```
Change the following line on *<n_studente>*-data-2:
```
#node.name: node-1
```
to
```
node.name: <n_studente>-data2
```
Change the following line on *<n_studente>*-data-1:
```
#node.attr.rack: r1
```
to
```
node.attr.temp: hot
```
Change the following line on *<n_studente>*-data-2:
```
#node.attr.rack: r1
```
to
```
node.attr.temp: warm
```
Add the following lines on *<n_studente>*-master:
```
node.master: true
node.data: false
node.ingest: false
node.ml: false
```
Add the following lines on *<n_studente>*-data1:
```
node.master: false
node.data: true
node.ingest: true
node.ml: false
```
Add the following lines on *<n_studente>*-data-2:
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
to the master IP
```
discovery.seed_hosts: ["<IP_master>"]
```
Change the following on each node:
```
#cluster.initial_master_nodes: ["node-1", "node-2"]
```
to your master assigned name
```
cluster.initial_master_nodes: ["<n_studente>-master"]
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

