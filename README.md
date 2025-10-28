# 🛡️ DevSecOps — Deployment of a Netflix Clone on AWS EKS

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

## 🛠️ High-Level Workflow

1. **Code Push** → GitHub triggers Jenkins pipeline
2. **CI Stages**:
   - JUnit testing
   - SonarQube static analysis (OWASP-aligned)
   - OWASP dependency scan
   - Trivy container scan
3. **Build & Publish**:
   - Docker image built and pushed to Docker Hub
4. **CD Stages**:
   - Kubernetes deployment via manifest
   - Ingress + LoadBalancer exposed
5. **GitOps Deployment**:
   - Argo CD syncs manifests from GitHub to cluster
   - Drift detection and rollback enabled
6. **Monitoring & Alerts**:
   - Prometheus scrapes metrics
   - Grafana dashboards
   - Slack/email notifications

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

## 📁 Folder Structure


## 🧠 Learning Note: Manual Infrastructure Provisioning

> **Note:** Terraform was intentionally **not used** for infrastructure provisioning in this project. All AWS resources — including EC2 instances, EKS cluster setup, security groups, and networking — were created **manually via the AWS Console and CLI**.

This decision was made to:

- Gain **deep visibility** into each stage of the infrastructure lifecycle  
- Understand **resource dependencies and configurations** without abstraction  
- Strengthen hands-on experience with AWS primitives before automating with IaC tools

Future iterations may incorporate Terraform or Pulumi for reproducibility and scalability, but this version prioritizes **manual clarity and platform intuition**.


