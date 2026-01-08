# 🚀 GitLab GitOps Deployment with Kubernetes (Minikube)


# ![git-cicd-diagram](imges/01.png)


This mini project is the **end-to-end process of building, pushing, and deploying a containerized application to a Kubernetes cluster using GitLab CI/CD and GitLab Kubernetes Agent (KAS)**.

The setup follows a **GitOps approach** using two repositories:
- **k8s-connection** → Manages GitLab Agent connection
- **k8s-data** → Application source code, CI/CD pipeline, and Kubernetes manifests

---

## 📁 Repository Structure

# ![git-cicd-diagram](imges/01.png)

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

1. `k8s-connection` (Link: https://gitlab.com/inwaseem63/k8s-connection)
2. `k8s-data`       (Link: https://gitlab.com/inwaseem63/k8s-data)

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


3 # ![git-cicd-diagram](imges/01.png)


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

4 # ![git-cicd-diagram](imges/01.png)


## 🔹 Step 04: Clone Both Repositories Locally

git clone https://gitlab.com/inwaseem63/k8s-data
git clone https://gitlab.com/inwaseem63/k8s-connection


## 🔹 Step 05: Build Docker Image (Sample V1)

Inside k8s-data, build the Docker image:

```bash
docker build -t sample:v1 .
```

5 # ![git-cicd-diagram](imges/01.png)


## 🔹 Step 06: Push Image to GitLab Container Registry

Login to GitLab Registry

docker login registry.gitlab.com

Build & Push Image
```bash
docker build -t registry.gitlab.com/<username>/k8s-data .
docker push registry.gitlab.com/<username>/k8s-data
```

5 # ![git-cicd-diagram](imges/01.png)

## 🔹 Step 07: Push Code to GitLab

```bash
git add .
git commit -m "Initial commit"
git push
```

## 🔹 Step 08: Create GitLab CI/CD Pipeline

```bash
Create .gitlab-ci.yml inside k8s-data repository.

stages:
  - build
  - deploy

build:
  image: docker:24.0.5-cli
  stage: build
  services:
    - docker:24.0.5-dind
  script:
    - echo "$CI_REGISTRY_PASSWORD" | docker login $CI_REGISTRY -u $CI_REGISTRY_USER --password-stdin
    - docker build -t $CI_REGISTRY_IMAGE:latest .
    - docker push $CI_REGISTRY_IMAGE:latest
```
Once committed, the pipeline will build and push the Docker image automatically.

6 # ![docker06](imges/docker06.png)


8 # ![k8s-dia08](imges/k8s-dia08.png)

## 🔹 Step 09: Deploy Application to Kubernetes

```bash
deploy_project:
  image:
    name: bitnami/kubectl:latest
    entrypoint: [""]
  stage: deploy
  script:
    - kubectl config get-contexts
    - kubectl config use-context "$KUBE_CONTEXT"
    - kubectl get nodes
    - kubectl apply -f k8s-files/
    - kubectl get pods
    - kubectl get svc
    - kubectl get nodes -o wide
```


7 # ![k8s-07](imges/k8s-07.png)


## 🔐 Fix Agent Access Issue (Important)

If the pipeline fails due to an agent connection issue, update the following file:
k8s-connection → .gitlab/agents/k8s-connection/config.yaml

```bash
ci_access:
  projects:
    - id: <username>/k8s-data
```

9 # ![cicd-09](imges/cicd-09.png)


Final OutPut
Docker images are built and pushed via GitLab CI/CD
GitLab Kubernetes Agent securely connects to Minikube
Application is deployed to Kubernetes using GitOps
No kubeconfig or secrets exposed in pipelines
Commit and push the change.

10 # ![output10](imges/output10.png)



