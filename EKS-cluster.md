## 🚀 EKS Cluster Creation with `eksctl`

Provisioned a new Amazon EKS cluster with public endpoint access and managed node group using `eksctl`.

### 🔧 Prerequisites
- AWS CLI configured (`aws configure`)
- `eksctl` installed via Homebrew:
  ```bash
  brew tap weaveworks/tap
  brew install weaveworks/tap/eksctl

Cluster Creation Command

eksctl create cluster \
  --name netflix-dev \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed \
  --ssh-access \
  --ssh-public-key nancy-devops-key \
  --with-oidc \
  --version 1.29


✅ Outcome
Cluster name: netflix-dev

Region: us-east-1

Node group: standard-workers (2 × t3.medium)

Public endpoint access enabled

IAM identity auto-mapped to system:masters

OIDC provider configured for IRSA

Post-Creation Verification
aws eks update-kubeconfig --name netflix-dev --region us-east-1
kubectl get nodes
kubectl get svc
kubectl get pods --all-namespaces


Notes
SSH key pair nancy-devops-key must exist in EC2 before cluster creation

Amazon Linux 2 AMIs will be deprecated after Nov 2025 — future clusters should use AL2023 or Bottlerocket

## EKS Cluster Teardown Process: 
## 🧹 EKS Cluster Teardown with `eksctl`

Cleanly deleted the `netflix-dev` EKS cluster and associated resources using `eksctl`.

### 🔧 Prerequisites
- `eksctl` installed and configured
- AWS CLI authenticated (`aws configure`)

### 🗑️ Cluster Deletion Command
```bash
eksctl delete cluster \
  --name netflix-dev \
  --region us-east-1


Outcome
Deleted EKS control plane

Removed managed node group (standard-workers)

Cleaned up associated CloudFormation stacks

Preserved VPC and key pair (nancy-devops-key) for future reuse

🧠 Notes
If custom VPC or IAM roles were created manually, delete them separately via AWS Console

Always verify teardown completion in CloudFormation and EC2 dashboards

## ✅ Post-Creation Checklist for EKS Cluster (`netflix-dev`)

After successful provisioning with `eksctl`, verify access and prepare the cluster for workloads.

### 🔍 1. Update kubeconfig
```bash
aws eks update-kubeconfig --name netflix-dev --region us-east-1


2. Verify cluster access
kubectl get nodes
kubectl get svc
kubectl get pods --all-namespaces
Expected output:

2 × t3.medium nodes in Ready state

Core services like kube-dns running in kube-system

🔐 3. Confirm IAM mapping
Your IAM identity is auto-mapped to system:masters via eksctl, allowing full kubectl access.

4. Deploy a sample workload (optional)

kubectl create deployment hello-k8s --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-k8s --type=LoadBalancer --port=80
kubectl get svc
