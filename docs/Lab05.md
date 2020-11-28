# Lab05: Working with Logstash

### 1. Install Logstash with default settings:

Import the Logstash key:
```
 rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```
Add the Logstash repo:
```
 vi /etc/yum.repos.d/logstash.repo

 [logstash-6.x]
 name=Elastic repository for 6.x packages
 baseurl=https://artifacts.elastic.co/packages/6.x/yum
 gpgcheck=1
 gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
 enabled=1
 autorefresh=1
 type=rpm-md
 ```
Install Logstash
```
 yum install logstash -y
```
Enable and start Logstash:
```
 systemctl enable logstash
 systemctl start logstash
 ```