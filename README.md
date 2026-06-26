
# GitOps with Argo CD on Docker Desktop Kubernetes

## Project Overview

This project demonstrates a complete beginner-friendly GitOps workflow using:

- FastAPI
- Docker
- Docker Hub
- Kubernetes (Docker Desktop)
- Argo CD
- GitHub

The objective is to understand how Git becomes the **single source of truth** and how Argo CD continuously synchronizes the Kubernetes cluster with the Git repository.

---

# Architecture

```text
Developer
    │
    ▼
FastAPI Code
    │
    ▼
Docker Build
    │
    ▼
Docker Hub
    │
    ▼
Update deployment.yaml
    │
    ▼
Git Push
    │
    ▼
GitHub Repository
    │
    ▼
Argo CD
    │
    ▼
Kubernetes
    │
    ▼
Running Application
```

---

# Technology Stack

| Component | Purpose |
|-----------|---------|
| FastAPI | REST API |
| Docker | Containerization |
| Docker Hub | Image Registry |
| Kubernetes | Container Orchestration |
| Argo CD | GitOps Continuous Delivery |
| GitHub | Source of Truth |
| kubectl | Kubernetes CLI |

---

# Project Structure

```text
hello-gitops/
├── app/
│   ├── app.py
│   ├── Dockerfile
│   └── requirements.txt
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
└── README.md
```

---

# Workflow

## 1. Develop

Modify the FastAPI source code.

## 2. Build

```bash
docker build -t hello-gitops:v1 .
```

## 3. Push Image

```bash
docker tag hello-gitops:v1 <dockerhub-user>/hello-gitops:v1
docker push <dockerhub-user>/hello-gitops:v1
```

## 4. Update Kubernetes Manifest

```yaml
image: <dockerhub-user>/hello-gitops:v1
```

## 5. Commit

```bash
git add .
git commit -m "Deploy v1"
git push
```

## 6. Argo CD

Argo CD detects the Git change, compares Git with the cluster, and synchronizes automatically.

---

# Why GitOps?

Traditional Deployment:

```text
Developer
   │
kubectl apply
   │
Kubernetes
```

GitOps:

```text
Developer
   │
git push
   │
GitHub
   │
Argo CD
   │
Kubernetes
```

Benefits:

- Declarative deployments
- Git is the source of truth
- Automatic synchronization
- Self-healing
- Easy rollback
- Complete audit history

---

# Self-Healing Demo

Git:

```yaml
replicas: 2
```

Manual change:

```bash
kubectl scale deployment hello-gitops --replicas=5
```

Argo CD detects drift and restores replicas back to **2**.

---

# Rolling Update Demo

1. Update FastAPI code.
2. Build and push image v2.
3. Update deployment.yaml image tag.
4. Commit and push.

Argo CD performs a rolling update automatically.

---

# Rollback Demo

```bash
git revert <commit-id>
git push
```

Argo CD deploys the previous version automatically.

---

# Common Issues

## ImagePullBackOff

Cause:
- Wrong image name
- Image not pushed
- Private repository

Fix:
- Verify Docker Hub image exists.
- Update deployment.yaml.
- Commit and push.

## OutOfSync

Cause:
Git repository differs from the cluster.

Fix:
Argo CD synchronizes automatically or manually.

## Degraded

Cause:
Pods unhealthy, image errors, crash loops, failed probes.

---

# Useful Commands

```bash
kubectl get pods
kubectl get svc
kubectl get deployments
kubectl describe deployment hello-gitops
kubectl logs <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

# Learning Outcomes

After completing this project you should understand:

- Docker image lifecycle
- Kubernetes Deployment and Service
- GitOps principles
- Argo CD synchronization
- Self-healing
- Rolling updates
- Rollbacks
- Git as the source of truth

---

# Future Enhancements

- GitHub Actions CI
- Helm
- Kustomize
- Secrets Management
- AWS EKS
- Monitoring with Prometheus & Grafana


<img width="1917" height="1079" alt="Screenshot 2026-06-26 152106" src="https://github.com/user-attachments/assets/96377426-7337-4c51-a83f-48c85629ac58" />

