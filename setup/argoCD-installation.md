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
  ## Exposed via Ingress
  export ARGOCD_SERVER=$(kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname')
  
  # print the end point
  echo $ARGOCD_SERVER 