# Installing Hashicorp Vault on kubernetes-cluster using helm charts

## 1. Download helm for linux
```
wget https://get.helm.sh/helm-v3.6.3-linux-amd64.tar.gz
tar -zxvf helm-v3.6.3-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
```

## 2. Add hashicorp helm repository
```
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo list
```

## 3. Installing hashicorp vault
```
helm -n my-vault install --create-namespace -g hashicorp/vault
```
Now hashicorp vault still not ready because we need to initialize vault and unseal it

## 4. Listing pods in my-vault namespace
```
kubectl get pods -n my-vault
```

## 5. Getting interactive terminal of hashicorp vault
```
kubectl -n my-vault exec -it "pod-name" -- /bin/sh
```

## 6. Vault status
we can check vault status by 
```
vault status
```

## 7. Initialising vault
```
vault operator init
```
**Copy keys and token somewhere you can't once you lost**

## 8. Unseal Vault
```
vault operator unseal
```
execute this 3 times and paste your 3 different keys
