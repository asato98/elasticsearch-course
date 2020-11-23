# Lab04: Charts and values.yaml

1. Fetch again the wordpress chart
```
:~$ helm fetch --untar bitnami/wordpress
:~$ ls
```
for starting, the wordpress chart needs a persistent volume provisioner,
now for testing we can install a local PV provisioner directly from a Helm repo

```
:~$ helm repo add rimusz https://charts.rimusz.net
:~$ helm search repo hostpath
NAME                       	CHART VERSION	APP VERSION	DESCRIPTION                                       
rimusz/hostpath-provisioner	0.2.5        	v0.2.2     	hostpath-provisioner is an automatic provisione...
```
install the "hostpath-provisioner" (in production you need a cloud PV provisioner)

```
:~$ helm install training-hp  rimusz/hostpath-provisioner
 NAME: training-hp
LAST DEPLOYED: Fri Nov 29 10:38:11 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The Hostpath Provisioner service has now been installed.

A storage class named 'hostpath' has now been created
and is available to provision dynamic volumes.
```
check the hostpath storageclass has been created
```
:~$kubectl get storageclasses
NAME                 PROVISIONER   AGE
hostpath (default)   hostpath      2m19s
```
2. for testing now you need to configure the wordpress chart for claiming a PV through hostpath provisioner and setting the service in NodePort type by modifying the values.yaml

```
:~$ vi ./wordpress/values.yaml
```
in the line .206 change de value and remove the hashtag for validate the line "storageClass: "hostpath"

repeat the previous step step for the line .332

on the line .214  change the Service type from LoadBalancer in NodePort.
Then save and exit

install the wordpress chart locally

```
:~$ helm install testing-wp ./wordpress
<output_omitted>
```
check the PV and other resources  are been created
```
:~$ kubectl get pv 
:~$ kubectl get pod
```
you can see the testing-wp pods are up and running, now check the application
```
:~$ kubectl get svc
:~$ curl http://<nodeIP>:<NodePort>
```
the wordpress welcome page will be printed.

you can also pass a specific created values.yaml file during the command

```
:~$ mkdir custom-values
:~$ cp ./wordpress/values.yaml ./custom-values/wp-custom.yaml
:~$ helm install custom-wp  --values=./custom-values/wp-custom.yaml bitnami/wordpress
:~$ kubectl get svc
```
you can see a new release installed directly from bitnami repo, with a custom values passed.




3. cleanup your environment
```
:~$ helm delete testing-wp  custom-wp
```






















