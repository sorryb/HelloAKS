#0. Install Kubectl
az aks install-cli

#1. Build Docker image
docker build . -t helloaks:0.0.1

docker tag helloaks:0.0.1 helloaksazurecontainerregistry.azurecr.io/hello-aks:0.0.1


#2. Login to existing Azure Container Registry : HelloAKSAzureContainerRegistry  from "myprojects" group

az acr login -n HelloAKSAzureContainerRegistry
												az login 
												Login Succeeded

docker push helloaksazurecontainerregistry.azurecr.io/hello-aks:0.0.1

az acr repository list -n helloaksazurecontainerregistry

# re-tag the  Docker Image
docker tag hello-aks:0.0.1 HelloAKSAzureContainerRegistry.azurecr.io/hello-aks:0.0.1

# 4. Login to existing Kubernetes AKS Cluster  : HelloAKSKubeCluster   from "myprojects" group

	# download cluster configuration for kubectl from "myprojects" group
	az aks get-credentials  -n HelloAKSKubeCluster -g myprojects

	# verify kubectl context
	kubectl config get-contexts
	# prints all cluster contexts

	# if kubectl does not point to HelloAKSKubeCluster, switch context
	kubectl config use-context HelloAKSKubeCluster

#Integrate an existing ACR with existing AKS clusters by supplying valid values for acr-name or acr-resource-id as below.
az aks update -n HelloAKSKubeCluster -g myprojects --attach-acr HelloAKSAzureContainerRegistry

# 5. Deploy artifacts to AKS
kubectl apply -f pod.yml
kubectl apply -f service.yml

	# wait for service to receive its external IP
	kubectl get svc -w
											NAME         TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
											hello-aks    LoadBalancer   10.0.94.52   20.71.74.14   8088:30107/TCP   63s
											kubernetes   ClusterIP      10.0.0.1     <none>        443/TCP          69m
# get pods 
kubectl get pods -o wide

# get  endpoints
kubectl get endpoints

curl http://20.71.74.14:8088/weatherforecast 


kubectl delete pod first-netcore-app-pod




































taken from "six steps ..."

