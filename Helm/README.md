# Cloud Ninjas Production Platform – Helm Chart

<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/f5661e74-2f1b-43b9-8d46-9fcf74fe993e" />


This Helm chart deploys the full **Cloud Ninjas Production Platform** stack, including:
- **Frontend** 
- **Backend** (FAST API)
- **Database** (MySQL,Redis)

It is designed for easy deployment on any Kubernetes cluster using Helm 3.

---

## 📁 Repository Structure

```
Helm/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── database-statefulset.yaml
│   ├── database-service.yaml
│   └── ...
└── README.md
```

---

## 🚀 Quick Start

### Prerequisites
- [Kubernetes cluster](https://kubernetes.io/) (v1.20+)
- [Helm 3](https://helm.sh/docs/intro/install/)
- `kubectl` configured to communicate with your cluster

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/MohamedMahmoudDesouky/cloud-ninjas-production-platform.git
   cd cloud-ninjas-production-platform/Helm
   ```

2. **Install the Helm chart**:
   ```bash
   helm install cloud-ninjas . --namespace cloud-ninjas --create-namespace
   ```

3. **Verify deployment**:
   ```bash
   kubectl get pods -n cloud-ninjas
   kubectl get svc -n cloud-ninjas
   ```

4. **Access the application**:
   - Frontend: `http://<EXTERNAL-IP-FRONTEND>`
   - Backend API: `http://<EXTERNAL-IP-BACKEND>:<PORT>`

   To get the external IPs:
   ```bash
   kubectl get svc -n cloud-ninjas
   ```

### Uninstall

```bash
helm uninstall cloud-ninjas -n cloud-ninjas
```

---

## 🛠️ Configuration

All configurable values are defined in [`values.yaml`](./values.yaml). You can override them using:

```bash
helm install cloud-ninjas . \
  --set frontend.replicaCount=3 \
  --set database.postgresPassword=supersecret \
  -n cloud-ninjas
```

Or provide a custom values file:

```bash
helm install cloud-ninjas . -f my-values.yaml -n cloud-ninjas
```

---

## 🌐 Architecture Overview

<img width="8875" height="501" alt="Frontend-Backend-PostgreSQL-2026-01-03-183006" src="https://github.com/user-attachments/assets/16773412-c327-4f57-87a5-5b06b339ae82" />

```


## 📝 Notes

- By default, services are exposed via `LoadBalancer` type. In local clusters (e.g., Minikube), use `NodePort` or `port-forward`.
- Persistent storage for the database uses a `PersistentVolumeClaim`. Ensure your cluster supports dynamic provisioning or pre-provision volumes.
- Secrets (e.g., DB passwords) are generated if not provided—review `values.yaml` for security best practices.

