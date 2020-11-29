# Lab03: Encrypt the Elasticsearch Transport Network


You are the administrator for a 3-node Elasticsearch cluster that has been used so far for ad-hoc, non-sensitive data analysis. However, the organization would like to leverage the analysis capabilities of Elasticsearch with some more sensitive data. Therefore, you will need to secure the cluster by encrypting the transport network and enabling user authentication so that any sensitive data will not be publicly available to anyone with network access.

The security team has already created a PKCS#12 certificate package for you to use with your Elasticsearch cluster. This package has already been deployed at /etc/elasticsearch/certificate.p12 and should be used to encrypt the transport network with certificate-level verification. Once the transport network is encrypted, you will need to set the built-in user passwords, using the elasticsearch-setup-passwords utility, to the following:
```
+------------------------+----------------------------+
| User                   | Password                   |
+------------------------+----------------------------+
| elastic                | elastic_566                |
+------------------------+----------------------------+
| apm_system             | apm_system_566             |
+------------------------+----------------------------+
| kibana                 | kibana_566                 |
+------------------------+----------------------------+
| logstash_system        | logstash_system_566        |
+------------------------+----------------------------+
| beats_system           | beats_system_566           |
+------------------------+----------------------------+
| remote_monitoring_user | remote_monitoring_user_566 |
+------------------------+----------------------------+
```



### 1. Configure transport network encryption.

Using the Secure Shell (SSH), log in to each node as cloud_user via the  IP address.

on Master Node become the root user with:
```
sudo su -
```
move to the elasticsearch dir and create certs dir
```
cd /etc/elasticsearch/
mkdir certs
cd certs
```
Create Certificate for KeyPairing that reflect your cluster name:
```
/usr/share/elasticsearch/bin/elasticsearch-certutil cert --name cluster-<n_studente> --out /etc/elasticsearch/certs/cluster-<n_studente>
```
Verify the certificate and set the permission:
```
ls -l
chmod 640 cluster-<n_studente>
```


you need to retrive ip addresses of your data nodes:

on your data nodes
```
ifconfig | grep "inet"
```
```
inet <your_ip_address>  netmask 255.255.255.0  broadcast 10.10.10.255
```
copy your certificate with scp on other nodes, when prompted enter password **DesotechKube1!**
```
scp cluster-<n_stduente> student@<your_data1_ip_address>:/home/student
```
```
scp cluster-<n_stduente> student@<your_data2_ip_address>:/home/student
```
```
cluster-<n_studente>                                              100% 3455     3.4MB/s   00:00
```
connect to **data1** and **data2** nodes and change owner, permission and path for the cert file
```
sudo su -
```
```
mkdir /etc/elasticsearch/certs
```
```
chown root:elasticsearch /home/student/cluster-<n_studente> 
```
```
mv /home/student/cluster-<n_studente> /etc/elasticsearch/certs
```
Change permissions:
```
chmod 640 /etc/elasticsearch/certs/cluster-<n_studente>
```



**Add the following to /etc/elasticsearch/elasticsearch.yml on each node**:
```
# --------------------------------- Security -----------------------------------
#
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.keystore.path: certs/cluster-<n_studente>
xpack.security.transport.ssl.truststore.path: certs/cluster-<n_studente>
```
### 2. Restart Elasticsearch.

Restart Elasticsearch with:
```
systemctl restart elasticsearch
```
### 3. Set the password for each built-in user.

Set the built-in user passwords using the elasticsearch-setup-passwords utility on the master-1 node:
```
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
```
Use the following passwords:
```
User: elastic
Password: elastic_566

User: apm_system
Password: apm_system_566

User: kibana
Password: kibana_566

User: logstash_system
Password: logstash_system_566

User: beats_system
Password: beats_system_566

User: remote_monitoring_user
Password: remote_monitoring_user_566
```
test your password:
```
curl localhost:9200/_cat/nodes?v
```
```
{"error":{"root_cause":[{"type":"security_exception","reason":"missing authentication credentials for REST request [/_cat/nodes?v]","header":{"WWW-Authenticate":"Basic realm=\"security\" charset=\"UTF-8\""}}],"type":"security_exception","reason":"missing authentication credentials for REST request [/_cat/nodes?v]","header":{"WWW-Authenticate":"Basic realm=\"security\" charset=\"UTF-8\""}},"status"
```
try again with user-pass
```
curl localhost:9200/_cat/nodes?v -u elastic
```
and type elastic password.

### 4. Securize Kibana


Now cluster is securized and Kibana needs a user password configuration
```
vim /etc/kibana/kibana.yml
```
change the following lines:

```
#elasticsearch.username: "kibana"
#elasticseaerch.password: "pass"
```
in

```
elasticsearch.username: "kibana"
elasticseaerch.password: "kibana_566"
```
Restart kibana service:

```
systemctl restart kibana
```
wait kibana startup and try to connect with yor master_ip:8080

Then enter credential and enter at "elastic" user with relative password "elastic_566".
