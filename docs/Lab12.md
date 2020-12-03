# Lab12: Use filebeats under Kubernetes




### 1. deploy filebeat on Kubernetes
**On your Data1 node**

download filebeat manifest

```
curl -O https://raw.githubusercontent.com/nabilsato/elasticstack/main/filebeat-kubernetes.yaml
```
modify the Daemonset object to match your cluster configuration on the manifest:

```
vi filebeat-kubernetes.yaml
```
```
env:
        - name: ELASTICSEARCH_HOST
          value: <IP_master>
        - name: ELASTICSEARCH_PORT
          value: "9200"
        - name: ELASTICSEARCH_USERNAME
          value: elastic
        - name: ELASTICSEARCH_PASSWORD
          value: elastic_566
```

then apply it:
```
kubectl apply -f filebeat-kubernetes.yaml
```

Now lets go to kibana and discover logs on your browser:
```
<master_IP>:8080
```

### 1. enable metricbeat module for Kubernetes monitoring


```
metricbeat modules enable kubernetes

metricbeat setup
```

Now lets go to kibana and discover k8s metrics on your browser:
```
<master_IP>:8080
```
