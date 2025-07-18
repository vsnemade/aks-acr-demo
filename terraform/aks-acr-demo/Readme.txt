This will create AKS Cluster that can pull images from ACR

Main challenge is to let AKS pull images from ACR. But keeps getting Unauthorized Error. 
How we solve it?
#Attach AKS to ACR
az aks update --name aks-demo-cluster --resource-group rg-aks-demo --attach-acr acrdemoregistryvish

#Login to ACK to push image.
az acr login --name acrdemoregistryvish

#Build Image
docker build -t acrdemoregistryvish.azurecr.io/aks-acr-demo:1.0 .

#Push Image
docker push acrdemoregistryvish.azurecr.io/aks-acr-demo:1.0

#Get the AKS credentials to run commands to this AKS
az aks get-credentials --resource-group rg-aks-demo --name aks-demo-cluster
 
#Deploy the pod 
kubectl apply -f deployment.yaml

#Deploy the service
kubectl apply -f service.yaml

#To see pod logs
aks-acr-deployment-6ffbdcf677-th4kn