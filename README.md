# kops-kubernetes-cluster
## 1. Launch Linux EC2 instance in AWS (Kubernetes Client)
## 2. Create and attach IAM role to EC2 Instance.
Kops need permissions to access
-	S3
-	EC2
-	VPC
-	Route53
-	Autoscaling
	etc..
## 3. Install Kops on EC2
```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```
## 4. Install kubectl
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

## 5. Create S3 bucket in AWS
S3 bucket is used by kubernetes to persist cluster state, lets create s3 bucket using aws cli Note: Make sure you choose bucket name that is unique accross all aws accounts

``` aws s3 mb s3://devopskops.in.k8s --region ap-south-1 ```

## 6. Configure environment variables.
Open .bashrc file

```	vi ~/.bashrc  ````
Add following content into .bashrc, you can choose any arbitary name for cluster and make sure buck name matches the one you created in previous step.
```
export KOPS_CLUSTER_NAME=devopskops.k8s.local
export KOPS_STATE_STORE=s3://devopskops.k8s.in
```
Then running command to reflect variables added to .bashrc
``` ource ~/.bashrc  ```
	
## 7. Create ssh key pair
This keypair is used for ssh into kubernetes cluster

``` ssh-keygen 
kops create secret --name devopskops.k8s.local sshpublickey admin -i ~/.ssh/id_rsa.pub
```

## 8. Create a Kubernetes cluster definition.
```
kops create cluster \
--state=${KOPS_STATE_STORE} \
--node-count=2 \
--master-size=t3.medium \
--node-size=t3.medium \
--zones=ap-south-1a,ap-south-1b \
--name=${KOPS_CLUSTER_NAME} \
```

## 9. Create kubernetes cluster
``` kops update cluster --yes --admin ```
Above command may take some time to create the required infrastructure resources on AWS. Execute the validate command to check its status and wait until the cluster becomes ready

``` kops validate cluster ```
For the above above command, you might see validation failed error initially when you create cluster and it is expected behaviour, you have to wait for some more time and check again.

## 10. Update Nodes and Master in the cluster
We can change numner of nodes and number of masters using following commands

we can list kops instances using
``` kops get instances ```

and instancegroups using
``` kops get instancegroups ```

```   kops edit ig <instancegroup-name> ```  it will open vi editor you can change values based on your requirements
After editing instancegrous apply below command
	
 ```  kops update cluster --yes  ```
## 11. Destroy the kubernetes cluster
``` kops delete cluster --name devopskops.k8s.local --yes ```



# Installing Grapaha,Kubernetes-dashboard and Prometheus in kops kubernetes cluster

## 1. Install git 
```
sudo yum install git -y 
```

## 2. Clone Repository
```
git clone https://github.com/THARAK-RAM1/kops-kubernetes-cluster 
```
## 3. Change to kops-kubernetes-cluster directory
```
cd kops-kubernetes-cluster
```

## 4. Install kubernetes-dashboard
```
kubectl create -f kubernetes-dashboard/
```

## 5. Install Prometheus
```
kubectl create -f prometheus
```

## 6. Install Node exporter 
```
kubectl create -f nodeexporter
```

## 7. Install Graphana
```
kubectl create -f grafana
```

All services have been created 
You can list kubernetes using 
```
kubectl get svc -A
```
Now you can access the required service with it's <loadbalancer ip>:<port>
