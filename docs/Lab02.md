# Lab02: Helm Repositories and Charts

1. We can search helm WordPress charts in different multiple repositories:
```
:~$ helm search repo
:~$ helm search hub wordpress
```
```
URL                                               	CHART VERSION	APP VERSION	DESCRIPTION                                       
https://hub.helm.sh/charts/bitnami/wordpress      	8.0.1        	5.3.0      	Web publishing platform for building blogs and ...
https://hub.helm.sh/charts/presslabs/wordpress-...	v0.6.3       	v0.6.3     	Presslabs WordPress Operator Helm Chart           
https://hub.helm.sh/charts/presslabs/wordpress-...	v0.7.3       	v0.7.3     	A Helm chart for deploying a WordPress site on ...
```
Adding the bitnami repository
```
:~$ helm repo add bitnami https://charts.bitnami.com
"bitnami" has been added to your repositories
```
Search the WordPress chart in the bitnami added repository 
```
:~$ helm search repo wordpress
```
```
NAME             	CHART VERSION	APP VERSION	DESCRIPTION                                       
bitnami/wordpress	8.0.1        	5.3.0      	Web publishing platform for building blogs and ..
```

Install WordPress chart
```
:~$ helm install training-wp bitnami/wordpress
```
```
NAME: training-wp
LAST DEPLOYED: Mon Nov 25 13:41:12 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the WordPress URL:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w training-wp-wordpress'
  export SERVICE_IP=$(kubectl get svc --namespace default training-wp-wordpress --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
  echo "WordPress URL: http://$SERVICE_IP/"
  echo "WordPress Admin URL: http://$SERVICE_IP/admin"

2. Login with the following credentials to see your blog

  echo Username: user
  echo Password: $(kubectl get secret --namespace default training-wp-wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)
```
2. Now check if your resources are deployed (Note: your pods should be pending for the persistent volume claim)
```
:~$ kubectl get deployment
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
training-wp-wordpress   0/1     1            0           111m
```
```
:~$ kubectl get pod
NAME                                    READY   STATUS    RESTARTS   AGE
training-wp-mariadb-0                   0/1     Pending   0          113m
training-wp-wordpress-c86f8fb6f-sr5m5   0/1     Pending   0          113m
```
```
:~$ kubectl get service
NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP                      165m
training-wp-mariadb     ClusterIP      10.105.146.97   <none>        3306/TCP                     115m
training-wp-wordpress   LoadBalancer   10.96.75.184    <pending>     80:31007/TCP,443:32488/TCP   115m
```
```
:~$ kubectl get configmap
NAME                        DATA   AGE
training-wp-mariadb         1      116m
training-wp-mariadb-tests   1      116m
```
3. Deploy another release 

```
:~$ helm install training-wp2 bitnami/wordpress
NAME: training-wp2
LAST DEPLOYED: Mon Nov 25 15:53:02 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
<output_omitted>
```
List your helm deployments
```
:~$ helm ls
```
Now you can see two helm deployments
Check that the kubernetes resources were created for the last training-wp2 deployment (point 2)

See the history of the release
```
:~$ helm history training-wp2
```
verify the service related to the training-wp2 release
```
:~$ kubectl get svc
NAME             TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
kubernetes       ClusterIP      10.96.0.1        <none>        443/TCP                      8d
training-wp2-mariadb     ClusterIP      10.106.239.128   <none>        3306/TCP                     97m
training-wp2-wordpress   LoadBalancer   10.97.119.116    <pending>     80:30385/TCP,443:31943/TCP   97m
```
change a Value to verfy upgrade process:

```
:~$ vi wordpress/values.yaml
```
change the line n 250 (the service port) from 80 to 90, then save and exit.
```
service:
  type: LoadBalancer
  ## HTTP Port
  ##
  port: 90
  ## HTTPS Port
  ##

```

Update your release
```
:~$ helm upgrade training-wp2 bitnami/wordpress
```
Then
```
:~$ helm history training-wp2
```
check the service with:
```
:~$ kubectl get svc
```
```
:~$ helm ls
```
You can see under the REVISION field the update count,
Now rollback to the first REVISION
```
:~$ helm rollback training-wp2 1
```
verify the service port:
```
:~$ kubectl get svc
```
Then
```
:~$ helm history training-wp2
REVISION	UPDATED                 	STATUS    	CHART           	APP VERSION	DESCRIPTION     
1       	Thu Jul  9 16:59:36 2020	superseded	wordpress-9.3.19	5.4.2      	Install complete
2       	Thu Jul  9 18:37:19 2020	superseded	wordpress-9.3.19	5.4.2      	Upgrade complete
3       	Thu Jul  9 18:45:56 2020	deployed  	wordpress-9.3.19	5.4.2      	Rollback to 1 
```
Under the DESCRIPTION field you can see "rollback to 1" for the last REVISION line 

4. Remove helm release
```
:~$ helm delete training-wp2 
release "training-wp2" uninstalled
```
Check that the release is uninstalled
```
:~$ helm ls
```
Now you are able to see only the first release
Check the release status
```
:~$ helm status training-wp
```