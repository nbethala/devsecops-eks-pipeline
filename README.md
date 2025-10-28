# 🛡️ DevSecOps — Deploy Netflix Clone on AWS EKS

## 📌 Executive Summary

This project implements a secure, automated CI/CD pipeline for deploying a Netflix-style movie browsing application on AWS infrastructure. It addresses the business challenge of delivering features rapidly without compromising security, reliability, or operational maturity.

By integrating vulnerability scanning, static analysis, credential hygiene, GitOps deployment, and observability, the pipeline enforces DevSecOps principles across the entire software lifecycle — from source control to production.

---

## 🧱 Architecture Overview

| Layer             | Tools & Services                                                                 |
|------------------|-----------------------------------------------------------------------------------|
| **Cloud Platform** | AWS (EC2, EKS, Security Groups, Load Balancer)                                  |
| **CI/CD**         | Jenkins, GitHub, Docker, Docker Hub                                              |
| **Security**      | Trivy, OWASP Dependency Check, SonarQube                                         |
| **Orchestration** | Kubernetes (Ubuntu-based master/worker nodes), Ingress Controller                |
| **GitOps**        | Argo CD with RBAC and TLS                                                        |
| **Monitoring**    | Prometheus, Node Exporter, Grafana                                               |
| **Notifications** | Slack, Email Integration                                                         |
| **App Layer**     | Netflix Clone with TMDB API integration                                          |

---

## 🛠️ High-Level Implementation Summary

### 🔄 CI/CD Pipeline (Jenkins)
- Source code pulled from GitHub on commit
- Jenkins pipeline stages:
  - Code checkout and build
  - JUnit testing
  - SonarQube static analysis (OWASP Top 10-aligned ruleset)
  - OWASP Dependency Check for third-party vulnerabilities
  - Trivy scan for container and IaC vulnerabilities
  - Docker image build and push to Docker Hub
- Secure credential injection via Jenkins credential store (no secrets in code)

### 🚀 CDP / GitOps Deployment (Argo CD)
- Argo CD deployed on EKS with TLS and RBAC
- Kubernetes manifests stored in Git and synced declaratively
- Drift detection and rollback enabled
- Argo CD UI used for visual sync status and deployment history

### ⚙️ Cloud Operations & Observability
- Infrastructure manually provisioned on AWS (EC2, EKS, Security Groups, Load Balancer)
- Prometheus and Node Exporter integrated for metrics collection
- Grafana dashboards for real-time visualization
- Slack and email notifications for pipeline status and alerts
- Audit trail maintained via archived Trivy and SonarQube reports

---

## 🔐 DevSecOps Integrations

| Tool                  | Purpose                                      | Enforcement Point         |
|-----------------------|----------------------------------------------|----------------------------|
| **Trivy**             | Container and IaC vulnerability scanning     | Jenkins pipeline           |
| **OWASP Dependency Check** | Third-party library vulnerability detection | Jenkins pipeline     |
| **SonarQube**         | Static code analysis + OWASP Top 10 rules    | Jenkins + Sonar dashboard |
| **RBAC + TLS**        | Secure access to Kubernetes and Argo CD      | Cluster + ingress config  |
| **Security Groups**   | Network-level access control in AWS          | EC2 + Kubernetes nodes     |

---

## 🚀 Argo CD Deployment

Argo CD manages declarative application deployments to Kubernetes, enabling GitOps workflows for consistency and traceability.

### Setup:
- Deployed via Kubernetes manifests
- Exposed via AWS LoadBalancer with TLS
- RBAC configured for secure access
- Admin password rotated and stored securely

### Workflow:
1. Git commit triggers Argo CD sync
2. Drift detection alerts on divergence
3. Rollbacks and history tracking enabled

---
## 📦 Technologies Used

- **Ubuntu 22.04**: Base OS for EC2 and Kubernetes nodes  
- **Jenkins**: CI/CD automation with plugins for SonarQube, Node.js, Kubernetes, OWASP  
- **Docker**: Containerization of app and tools  
- **Kubernetes**: Orchestration across master and worker nodes  
- **Prometheus & Grafana**: Monitoring and visualization  
- **Slack & Email**: Pipeline notifications  
- **TMDB API**: Movie data integration  
- **Node.js**: Runtime for app logic  
- **JDK**: Java tooling for Jenkins and SonarQube  

---

## 📁 Project Structure — devsecops-eks-pipeline

```
devsecops-eks-pipeline
├── LICENSE
├── README.md
├── backend
│   ├── Dockerfile
│   ├── public
│   ├── src
├── kubernetes
│   ├── deployment.yml
│   ├── node-service.yml
│   └── service.yml
├── setup
│   ├── argoCD-installation.md
│   └── eks-setup.md
└── troubleshoot.md
```

## 🧠 Learning Note: Manual Infrastructure Provisioning

> **Note:** Terraform was intentionally **not used** for infrastructure provisioning in this project. All AWS resources — including EC2 instances, EKS cluster setup, security groups, and networking — were created **manually via the AWS Console and CLI**.

This decision was made to:

- Gain **deep visibility** into each stage of the infrastructure lifecycle  
- Understand **resource dependencies and configurations** without abstraction  
- Strengthen hands-on experience with AWS primitives before automating with IaC tools

Future iterations may incorporate Terraform or Pulumi for reproducibility and scalability, but this version prioritizes **manual clarity and platform intuition**.


