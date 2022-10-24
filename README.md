# argo-spark-example
argo workflow spark logs example
Hello-world workflow logs get archived in the azure storage account but spark workflow logs do not.

1. kubectl create namespace freia
2. kubectl create namespace argo  
3. kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.4.1/install.yaml  
4. helm repo add spark-operator https://googlecloudplatform.github.io/spark-on-k8s-operator  
5. helm install my-release spark-operator/spark-operator --namespace spark-operator --set sparkJobNamespace=freia --create-namespace  
6. kubectl apply -f permissions-argo.yaml
7. Create azure blob storage account. Update storage account name and container name in workflow-controller-configmap.yaml.
8. kubectl create secret generic my-azure-storage-credentials \
  --from-literal "account-access-key=$(az storage account keys list -n storageaccountname --query '[0].value' -otsv)" -n argo
  Don't forget to update the storage account name here.
9. kubectl apply -f workflow-controller-configmap.yaml
10. kubectl apply -f spark-operator-kubernetes-dag.yaml