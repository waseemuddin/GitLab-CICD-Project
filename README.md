# 🚀 GitLab GitOps Deployment with Kubernetes (Minikube)

This repository documents the **end-to-end process of building, pushing, and deploying a containerized application to a Kubernetes cluster using GitLab CI/CD and GitLab Kubernetes Agent (KAS)**.

The setup follows a **GitOps approach** using two repositories:
- **k8s-connection** → Manages GitLab Agent connection
- **k8s-data** → Application source code, CI/CD pipeline, and Kubernetes manifests

---

## 📁 Repository Structure

.
├── k8s-connection
│ └── .gitlab/agents/k8s-connection/config.yaml
│
├── k8s-data
│ ├── Dockerfile
│ ├── .gitlab-ci.yml
│ └── k8s-files/
│ ├── deployment.yaml
│ ├── service.yaml
│ └── secret.yaml


---

## ✅ Prerequisites

- GitLab account
- Kubernetes cluster (any one):
  - Amazon EKS
  - Azure AKS
  - Google GKE
  - DigitalOcean Kubernetes
  - **Minikube (local Kubernetes)**
- Installed locally:
  - `kubectl`
  - `helm`
  - `docker`
  - `git`

---

## 🔹 Step 01: Create GitLab Repositories

Create **two repositories** in GitLab:

1. `k8s-connection`
2. `k8s-data`

---

## 🔹 Step 02: Install GitLab Kubernetes Agent

### Create Agent Configuration File

In the **k8s-connection** repository, create:

