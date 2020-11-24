# Lab02: Deploy and Configure Kibana for an Elasticsearch Cluster

### 1. Install Kibana on the master-1 node.

Using the Secure Shell (SSH), log in to the master-1 node as cloud_user via the public IP address.

Become the root user with:
```
sudo su -
```
Download the Kibana 7.6 RPM:
```
curl -O https://artifacts.elastic.co/downloads/kibana/kibana-7.6.0-x86_64.rpm
```
Install Kibana:
```
rpm --install kibana-7.6.0-x86_64.rpm
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
to
```
server.host: "10.0.1.101"
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

After Kibana has finished starting up, which may take a few minutes navigate to http://PUBLIC_IP_ADDRESS_OF_MASTER-1:8080 in your web browser and navigate to Dev Tools > Console.

Check the node status of the cluster via the console tool with:
```
GET _cat/nodes?v
```