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

``shell
.gitlab/agents/k8s-connection/config.yaml
``

> Leave the file **empty initially**. Configuration will be added later.

---

## 🔹 Step 03: Connect Kubernetes Cluster to GitLab

1. Go to **GitLab → Operate → Kubernetes Clusters**
2. Click **Connect a cluster**
3. Select **GitLab Agent**
4. Copy the **Agent Access Token**

Example:
``shell
glagent-xxxxxxxxxxxxxxxxxxxxxxxx
``


---

### Install Agent Using Helm

Run the following commands from your terminal:

```bash
helm repo add gitlab https://charts.gitlab.io
helm repo update

helm upgrade --install k8s-connection gitlab/gitlab-agent \
  --namespace gitlab-agent-k8s-connection \
  --create-namespace \
  --set config.token=<AGENT_TOKEN> \
  --set config.kasAddress=wss://kas.gitlab.com


