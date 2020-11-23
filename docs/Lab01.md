# Lab 01: Installing Helm with downloaded script

Download the script and run it locally
This script will download the latest version of helm binary and move it in the bin directory


```
 root@Master:~# curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
```
 output
```
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6617  100  6617    0     0  15347      0 --:--:-- --:--:-- --:--:-- 15352
```

Set permissions
```
root@Master:~# chmod 700 get_helm.sh
```
Run Script
```
root@Master:~# ./get_helm.sh
```
output
```
Downloading https://get.helm.sh/helm-v3.0.0-linux-amd64.tar.gz
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
```

Test installation
```
root@Master:~# helm info
```
```
The Kubernetes package manager

Common actions for Helm:

- helm search:    search for charts
- helm pull:      download a chart to your local directory to view
- helm install:   upload the chart to Kubernetes
- helm list:      list releases of charts
<output_omitted>
```
You are now ready to start using Helm.
