# cloud-ninjas-production-platform
<img width="1791" height="1068" alt="diagram-export-1-3-2026-5_15_03-PM" src="https://github.com/user-attachments/assets/e45ca446-9f6e-43e6-b230-f6c55310a882" />

# 🌩️ Cloud Ninjas Production Platform

> **A production-grade, cloud-native platform built with GitOps, Kubernetes, Helm, ArgoCD, and secure CI/CD pipelines**  
> 🔒 Security | 📊 Observability | 📈 Autoscaling | 🔄 GitOps

![CI Pipeline](https://github.com/MohamedMahmoudDesouky/cloud-ninjas-production-platform/workflows/Professional%20CI%20Pipeline%20(SonarCloud%20%2B%20Trivy%20%2B%20Smoke%20%2B%20CIS)/badge.svg)

This repository implements a full-stack cloud-native application platform using modern DevOps practices, infrastructure-as-code, and automated security & quality gates. The system includes **frontend**, **backend**, and **PostgreSQL database**, deployed on Kubernetes with full observability, autoscaling, and GitOps-driven continuous delivery.

---

## 🗂️ Repository Structure

```bash
cloud-ninjas-production-platform/
├── .github/workflows/       # CI/CD pipelines (GitHub Actions)
├── Helm/                    # Helm chart for templated Kubernetes deployment
├                                              
├── docker/                  
│   ├── frontend/           
│   ├── backend/             
│   └── database/│   ├── backend/            
│   └── README/│   ├── backend/             
│   └── docker-compose.yaml/            
│
├── k8s-final/               # Production-ready Kubernetes manifests
│   ├── *.yaml               # Deployments, Services, ConfigMaps, Secrets, PVCs
│   └── ci-final/            # Subset for smoke testing in CI
│
├── requirements.txt         # Python dependencies (for backend/testing)
├── sonar-project.properties # SonarCloud configuration
└── README.md
```

---

## 🚀 Deployment Options

You can deploy the platform in **two ways**:

### 1. ✅ **Helm Chart (Recommended for Production)**
- Templated, configurable, and idempotent
- Located in [`/Helm`](./Helm)
- Supports customization via `values.yaml`

**Install:**
```bash
helm install cloud-ninjas ./Helm --namespace cloud-ninjas --create-namespace
```

### 2. ✅ **Vanilla Kubernetes Manifests**
- Static YAML files for transparency and simplicity
- Located in [`/k8s-final`](./k8s-final)
- Ideal for learning or non-Helm environments

**Apply:**
```bash
kubectl apply -f k8s-final/
```

---

## 🛠️ CI/CD Pipeline Overview

The platform uses a **multi-stage GitHub Actions pipeline** defined in [`.github/workflows/ci.yml`](./.github/workflows/ci.yml):

| Stage | Purpose | Tools |
|------|--------|-------|
| **SonarCloud Scan** | Code quality & security | SonarCloud, Python |
| **Build & Scan** | Build Docker images + vulnerability scan | Docker, Trivy |
| **Smoke Test** | Validate deployment & health endpoint | Minikube, kubectl, curl |
| **CIS Benchmark** | Kubernetes node security compliance | kube-bench |
| **GitOps Sync** | Auto-commit new image tags to Git (triggers ArgoCD) | Git, sed |

> ✅ On successful pipeline completion, **ArgoCD** automatically syncs the updated manifests from `k8s-final/` to the live cluster (GitOps model).

---

## 📦 Services

| Component | Tech Stack | Port | Image |
|---------|----------|------|-------|
| **Frontend** | Nginx | 80 | `selconyt/fe:<tag>` |
| **Backend** | FastAPI | 8000 | `selconyt/be:<tag>` |
| **Database** | MySQL | 5432 | Official `MySQL` image |


---

## 🔒 Security & Compliance

- **Static Code Analysis**: SonarCloud for code smells, bugs, and vulnerabilities
- **Container Scanning**: Trivy scans every Docker image for CVEs
- **K8s Hardening**: CIS Benchmark (kube-bench) validates node compliance
- **Secrets Management**: Kubernetes Secrets (rotate in production with Vault/ExternalSecrets)
- **Least Privilege**: Services run as non-root users with restricted capabilities

---

## 📊 Observability (Design Notes)

While not fully implemented in this repo, the platform is **designed for observability**:
- Structured JSON logs in backend
- Health/liveness probes for all pods
- Metrics endpoints ready for Prometheus scraping
- Tracing-ready (OpenTelemetry-compatible)

> 🔜 Future: Integrate Prometheus, Grafana via Helm charts.

---

## 🧪 Local Development

### With Docker Compose
```bash
cd docker
docker-compose up --build
```
- Frontend: `http://localhost:3000`
- Backend: `http://localhost:8000`
- DB: `localhost:5432`

### With Minikube + k8s Manifests
```bash
minikube start
kubectl apply -f k8s-final/
minikube service frontend-service
```

---

## 🔄 GitOps with ArgoCD (Production)

1. ArgoCD is configured to watch this repo’s `k8s-final/` directory.
2. When the CI pipeline updates image tags and pushes to `main`, ArgoCD:
   - Detects the Git change
   - Syncs the new manifests to the cluster
   - Ensures desired state = actual state
