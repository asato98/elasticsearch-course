# Lab02: Deploy and Configure Kibana for an Elasticsearch Cluster


You are a system administrator who has been given access to a 3-node Elasticsearch cluster. You would like to install a Kibana instance on the master-1 node in order to better interact with Elasticsearch APIs using Kibana's Console tool. For this, you will need to install Kibana 7.6.0 from an RPM at https://artifacts.elastic.co/downloads/kibana/kibana-7.6.0-x86_64.rpm. Then, you will need to bind Kibana on the private IP address of the master-1 node and port 8080 in order to be able to access it remotely from the internet.

Once Kibana is up and running on the appropriate network address and port, you will need to navigate to http://PUBLIC_IP_ADDRESS_OF_MASTER-1:8080, access the console tool, and use it to interface with Elasticsearch.


### 1. Install Kibana on the master-1 node.

Using the Secure Shell (SSH), log in to the master-1 node as cloud_user via the public IP address.

Become the root user with:
```
sudo su -
```

update repo index
```
apt-get update
```

Install Kibana:
```
apt-get install kibana=7.6.0
```
Configure Kibana to start on system boot:
```
systemctl enable kibana
```
### 2. Configure Kibana.

Log in to the master-1 node and become the root user with:
```
sudo su -
```
Open the /etc/kibana/kibana.yml file:
```
vim /etc/kibana/kibana.yml
```
Change the following line:
```
#server.port: 5601
```
to
```
server.port: 8080
```
Change the following line:
```
#server.host: "localhost"
```
to the master IP
```
server.host: "<IP_master>"
```
### 3. Start Kibana.

Log in to the master-1 node and become the root user:
```
sudo su -
```
Start Kibana:
```
systemctl start kibana
```
### 4. Use Kibana's Console tool.

After Kibana has finished starting up, which may take a few minutes navigate to http://IP_ADDRESS_OF_MASTER:8080 in your web browser and navigate to Dev Tools > Console.

Check the node status of the cluster via the console tool with:
```
GET _cat/nodes?v
```