# Lab03: Charts in depth

Fetch the wordpress Chart in local directory
```
:~$ helm fetch bitnami/wordpress
```
```
:~$ls
wordpress-8.0.1.tgz
```

now fetch with decompression 
```
:~$ helm fetch --untar bitnami/wordpress
:~$ ls
wordpress  wordpress-8.0.1.tgz
:~$ rm -f wordpress-8.0.1.tgz
```
list the dependencies of the chart
```
:~$ helm dep list ./wordpress
```
it will print the mariadb dep.

inspect the chart directory of wordpress and delete the dependencies resources
```
:~$ ls ./wordpress
:~$ ls ./wordpress/charts
:~$ rm -rf ./wordpress/charts/mariadb
```
try install the chart locally
```
:~$ helm install training-wp-dep ./wordpress
Error: found in Chart.yaml, but missing in charts/ directory: mariadb

```

now resolv the dependencies

```
:~$ helm dep list ./wordpress
NAME   	VERSION	REPOSITORY                                       	STATUS 
mariadb	7.x.x  	https://kubernetes-charts.storage.googleapis.com/	missing
```
```
:~$ helm dep update ./wordpress
:~$ helm dep list ./wordpress
```
now the state of the dep is OK
install again
```
:~$ helm install training-wp-dep ./wordpress
```
the release will be installed

Clenup for the next lab
```
:~$ helm delete training-wp-dep
:~$ rm -rf ./wordpress
```