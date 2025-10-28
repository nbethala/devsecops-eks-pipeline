# ArgoCD Installation : use online documentation for latest version

### Deploy Application with ArgoCD
Install ArgoCD:

You can install ArgoCD on your Kubernetes cluster by following the instructions provided in the EKS Workshop documentation.

### Set Your GitHub Repository as a Source:

After installing ArgoCD, you need to set up your GitHub repository as a source for your application deployment. This typically involves configuring the connection to your repository and defining the source for your ArgoCD application. The specific steps will depend on your setup and requirements.

### Create an ArgoCD Application:

name: Set the name for your application.
destination: Define the destination where your application should be deployed.
project: Specify the project the application belongs to.
source: Set the source of your application, including the GitHub repository URL, revision, and the path to the application within the repository.
syncPolicy: Configure the sync policy, including automatic syncing, pruning, and self-healing.
Access your Application

To Access the app make sure port 30007 is open in your security group and then open a new tab paste your NodeIP:30007, your app should be running.

```
kubectl get pods
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml
  curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
  kubectl get namespace
  kubectl get all -argocd
  kubectl get all -n argocd
  kubectl patch svc argocd-server -n argocd   -p '{"spec": {"type": "LoadBalancer"}}'
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  kubectl create namespace prometheus-node-exporter
  helm install prometheus-node-exporter prometheus-community/prometheus-node-exporter --namespace prometheus-node-exporter
  kubectl get ns
```

  ### Exposed via Ingress

  ```
  export ARGOCD_SERVER=$(kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname')
  ```
  # print the end point
  echo $ARGOCD_SERVER 
