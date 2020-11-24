# Lab03: Encrypt the Elasticsearch Transport Network

### 1. Configure transport network encryption.

Using the Secure Shell (SSH), log in to each node as cloud_user via the public IP address.

Become the root user with:
```
sudo su -
```
Add the following to /etc/elasticsearch/elasticsearch.yml on each node:
```
# --------------------------------- Security -----------------------------------
#
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.keystore.path: certificate.p12
xpack.security.transport.ssl.truststore.path: certificate.p12
```
### 2. Restart Elasticsearch.

Start Elasticsearch with:
```
systemctl start elasticsearch
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