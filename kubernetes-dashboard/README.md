# Kubernetes-dashboard

## 1. List all services in kubernetes-cluster
```
kubectl get svc -A
```

## 2. Copy LoadBalancer address of kubernetes-dashboard service 
```
https://yourloadbalancerurl:443
```
## 3. Kubenetes-dashboard authentication
After accessing kubernets-dashboard url it will ask token and kubeconfig file for authentication
Run below command for token
```
kubectl describe secrets dashboard-admin -n kubernetes-dashboard
```
Copy the token and paste on kubernetes-dashboard console

Authenticated successfully , you can now use kubernetes-dashboard for kubernetes-cluster monitoring
