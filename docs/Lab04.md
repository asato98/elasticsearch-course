# Lab04: Encrypt the Elasticsearch Client Network


You are the administrator of a 3-node Elasticsearch cluster which is currently used for ad-hoc analysis of business data by your organization. The cluster has already been secured by encrypting the transport network and enabling user authentication but the organization would like you to enable client network encryption as well so that API requests can be obfuscated from anyone monitoring network traffic.

To accomplish this, you can use the existing PKCS#12 certificate package that is being used to encrypt the transport network to also encrypt the client network. It should be noted that this is a self-signed certificate and so it will not be globally trusted.


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
