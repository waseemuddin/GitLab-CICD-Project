# 🚀 GitLab GitOps Deployment with Kubernetes (Minikube)

This repository documents the **end-to-end process of building, pushing, and deploying a containerized application to a Kubernetes cluster using GitLab CI/CD and GitLab Kubernetes Agent (KAS)**.

The setup follows a **GitOps approach** using two repositories:
- **k8s-connection** → Manages GitLab Agent connection
- **k8s-data** → Application source code, CI/CD pipeline, and Kubernetes manifests

---

## 📁 Repository Structure

