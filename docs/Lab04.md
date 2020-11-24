# Lab04: Encrypt the Elasticsearch Client Network

### 1. Configure client network encryption.

Using the Secure Shell (SSH), log in to each node as cloud_user via the public IP address.

Become the root user with:
```
sudo su -
```
Add the following to /etc/elasticsearch/elasticsearch.yml on each node:
```
xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.keystore.path: certificate.p12
xpack.security.http.ssl.truststore.path: certificate.p12
```
### 2. Restart Elasticsearch.

Restart Elasticsearch with:
```
systemctl restart elasticsearch
```
