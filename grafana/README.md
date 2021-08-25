# Grafana

## 1. List all services in kubernetes-cluster
```
kubectl get svc -A
```

## 2. Copy LoadBalancer URL of grafana service 
```
https://yourloadbalancerurl:3000
```
## 3. Grafana authentication
After accessing Grafana url it will ask username and password for authentication
default username and password are 
username - admin
password - admin

Then Create new password

Authenticated successfully , you can now use Grafana for Metric visualisation


## 4.Add prometheus as a data source
Grafana is just a metric visualization tool it does't know where your metrics are stored so you need to add prometheus as a data source for retrieving metric from prometheus.

Add prometheus url in Grafana data source 
