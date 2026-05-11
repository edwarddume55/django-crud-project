# Django CRUD Platform (DevOps/SRE Project)

## 📌 Project Overview

This is a production-style Django REST API for a task management system, built as part of a DevOps/SRE learning journey.

The project has evolved beyond basic Docker setup into a Kubernetes-deployed microservice architecture with PostgreSQL, Ingress routing, Helm charts, and real-world debugging scenarios. It now includes **full observability** with Prometheus metrics and Grafana dashboards.

---

## 🏗️ Tech Stack

- Django 5.1+
- Django REST Framework
- PostgreSQL 15
- Docker & Docker Compose
- Kubernetes (Minikube)
- NGINX Ingress Controller
- Helm Charts
- Prometheus (Metrics Collection)
- Grafana (Visualization)
- GitHub Actions (CI/CD)
- Python 3.11

---

## 📁 Project Structure

```bash
django-crud-platform/
├── config/                 # Django project settings
├── tasks/                  # Tasks app (CRUD)
├── k8s/                    # Kubernetes manifests (legacy)
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   └── postgres-deployment.yaml
├── helm/                   # Helm charts
│   └── django-app/
│       ├── templates/
│       │   ├── deployment.yaml
│       │   ├── service.yaml
│       │   ├── ingress.yaml
│       │   └── secret.yaml
│       ├── Chart.yaml
│       └── values.yaml
├── monitoring/             # Prometheus/Grafana configs
│   ├── django-servicemonitor.yaml
│   └── django-dashboard.json
├── .github/workflows/
│   └── ci-cd.yml
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── manage.py
└── .env (not committed)

```

---

## 🚀 Features Implemented

## 🟢 Application Layer

- Django REST Framework CRUD API
- Task management endpoints
- PostgreSQL integration
- Health check endpoint (/health/)
- Prometheus metrics endpoint (/metrics)

## 🟢 Containerization

- Dockerized Django application
- Docker Compose setup with PostgreSQL
- Environment variable configuration
- Multi-stage build optimization

## 🟢 Kubernetes (Production Simulation)

- Deployment with multiple replicas
- PostgreSQL as separate deployment
- Kubernetes Service (ClusterIP)
- ConfigMaps & Secrets management
- Readiness & Liveness Probes
- Ingress Controller (NGINX)
- Custom domain routing (django.local)
- Helm Chart packaging and management

## 🟢 Observability (Prometheus + Grafana)
- Prometheus metrics collection (cluster + application)
- Grafana dashboards for visualization
- Django application metrics:
    - HTTP request rate & latency
    - Response status codes
    - Database query metrics

- Kubernetes infrastructure metrics:
    - CPU & Memory usage
    - Pod status & restarts
    - Node health
- ServiceMonitor for automatic metrics discovery
- Pre-built SRE dashboard with key panels

## 🟢 CI/CD (Automation)

- GitHub Actions pipeline
- Automated Docker image build on push
- Image tagging using commit SHA
- Push to Docker Hub registry
- Basic application validation (Django checks)

## 🟢 DevOps Debugging Experience

- Fixed CrashLoopBackOff issues
- Resolved PostgreSQL connection misconfiguration
- Debugged environment variable conflicts
- Fixed Ingress 503 service routing issues
- Resolved Helm dependency conflicts
- Handled ConfigMap/Secret ownership with Helm
- Fixed image pull policies and caching issues
- Handled Kubernetes networking and service discovery

---

## ⚙️ Local Setup (Docker Compose)

### 1. Clone repository

```bash
git clone https://github.com/edwarddume55/django-crud-platform.git
cd django-crud-platform
```

### 2. Create .env file

```bash
POSTGRES_DB=your_db
POSTGRES_USER=your_user
POSTGRES_PASSWORD=your_password
POSTGRES_HOST=db
POSTGRES_PORT=5432
DEBUG=True
SECRET_KEY=your_secret_key
```

### 3. Run with Docker Compose

```bash
docker compose up --build
```

- Access at: http://localhost:8000

---

## ☸️ Kubernetes Deployment (Minikube + Helm)

### 1. Start cluster

```bash
    minikube start --driver=docker
```

### 2. Enable Ingress

```bash
    minikube addons enable ingress
```

### 3. Install Prometheus Stack (Observability)
```bash
    # Add Helm repo
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update

    # Install kube-prometheus-stack
    helm install monitoring prometheus-community/kube-prometheus-stack

    # Verify pods are running
    kubectl get pods
    # Should see: prometheus, grafana, alertmanager
```
### 4. Deploy PostgreSQL manually (or let Helm manage it)

```bash
    # Manual PostgreSQL (for control)
    kubectl apply -f k8s/postgres-deployment.yaml
    kubectl apply -f k8s/postgres-service.yaml

```

### 5. Deploy Django app with Helm

```bash
   cd helm/django-app

    # Install with metrics-enabled image
    helm install django-app . --set image.tag=metrics

    # Verify deployment
    kubectl get pods
    kubectl get svc
    kubectl get ingress
```

### 6. Configure Prometheus to scrape Django
```bash
    # Create ServiceMonitor for Django
    kubectl apply -f monitoring/django-servicemonitor.yaml

    # Verify it's discovered
    kubectl get servicemonitor

```

### 7. Configure hosts file (Windows/macOS/Linux)

Add to /etc/hosts (Linux/macOS) or C:\Windows\System32\drivers\etc\hosts (Windows):

```bash
    192.168.49.2  django-app.local
```

Get Minikube IP if needed:

```bash
    minikube ip
```

---

### 8. Run database migrations

```bash
    kubectl apply -f k8s/migrate-job.yaml
```

### 9. Create superuser (optional)

```bash
    kubectl exec -it deployment/django-app -- python manage.py createsuperuser
```

---

## 🌍 Access the Application

Via Port-Forward (Development)

```bash
    kubectl port-forward service/django-app 8000:80
```

Then open: http://localhost:8000

Via Ingress (Production-like)

```bash
http://django-app.local/api/tasks/
http://django-app.local/admin/
http://django-app.local/health/
http://django-app.local/metrics 
```

Via Minikube Service

```bash
minikube service django-app
```

---

## 📊 Observability Stack
### Access Prometheus UI
```bash 
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090
```
Open: http://localhost:9090
#### Try these queries:
- django_http_requests_total_by_method_total
- django_db_queries_total
- rate(container_cpu_usage_seconds_total[5m])

### Access Grafana UI
```bash
    # Get Grafana admin password
    kubectl get secret monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 --decode

    # Port-forward Grafana
    kubectl port-forward svc/monitoring-grafana 3000:80
```
Open: http://localhost:3000 (Username: admin, Password: from above)

### Key Dashboard Panels
- CPU Usage -	rate(container_cpu_usage_seconds_total[5m])	- Cluster resource monitoring
- Memory Usage - container_memory_usage_bytes -	Memory utilization
- Request Rate - sum(rate(django_http_requests_total_by_method_total[5m])) - API traffic
- Request Latency - histogram_quantile(0.95, sum(rate(django_http_requests_latency_seconds_bucket[5m])) by (le))- Performance

## 🔄 CI/CD Pipeline

The project includes a GitHub Actions pipeline that automatically:

- Runs Django checks (python manage.py check)
- Builds Docker image
- Tags image with commit SHA
- Pushes image to Docker Hub

### 📦 Trigger

```bash
    git push origin main
```

### 📍 Pipeline Location

```bash
    .github/workflows/ci-cd.yml
```

### 🔧 Environment Secrets Required

- DOCKER_USERNAME - Docker Hub username
- DOCKER_PASSWORD - Docker Hub access token

---

## 🌐 API Endpoint

| Method | Endpoint         | Description            |
| ------ | ---------------- | ---------------------- |
| GET    | /api/tasks/      | List all tasks         |
| POST   | /api/tasks/      | Create a task          |
| PUT    | /api/tasks/<id>/ | Update a task          |
| DELETE | /api/tasks/<id>/ | Delete a task          |
| GET    | /health/         | Health check           |
| GET    | /admin/          | Django admin interface |
| GET	 | /metrics	        | Prometheus metrics     |
---

## 📦 Current Status

### ✅ Completed

- Django REST API
- Docker + Compose setup
- PostgreSQL integration
- Kubernetes deployment
- Helm chart packaging
- Service + ConfigMap + Secret management
- Ingress routing with custom domain
- CI/CD pipeline (GitHub Actions)
- Docker Hub registry integration
- Prometheus metrics collection
- Grafana dashboards
- ServiceMonitor configuration
- Production-like debugging experience

---

## 🐛 Debugged Issues

- CrashLoopBackOff - Missing gunicorn
- ImagePullBackOff - Image policy issues
- CreateContainerConfigError - Missing ConfigMaps
- PostgreSQL connection failures
- Helm secret ownership conflicts
- Ingress 503 service routing
- Service port mismatches
- Prometheus ServiceMonitor discovery

---

## 🔜 Next Steps

### 📊 Observability
- Loki (logs)

### ⚙️ Production Hardening

- Horizontal Pod Autoscaler (HPA)
- Resource limits & requests
- Pod Disruption Budgets
- Network Policies (RBAC)
- Secrets encryption
- TLS/HTTPS with cert-manager

### 🚀 Advanced CI/CD

- Kubernetes auto-deployment
- Helm-based releases
- Rollback strategies
- Blue/Green deployment
- ArgoCD GitOps

---

📦 Helm Improvements

- PostgreSQL as Helm dependency
- Environment-specific values files
- Helm hooks for migrations

---

## 🚀 Goal

- This project demonstrates a real-world DevOps/SRE system lifecycle:

```bash
Application Development
    ↓
Containerization (Docker)
    ↓
Orchestration (Kubernetes)
    ↓
Networking & Ingress
    ↓
Package Management (Helm)
    ↓
CI/CD Automation
    ↓
Observability (Prometheus + Grafana)
    ↓
Production Readiness
```

---

### 🧠 Key Learning Outcomes

- ✅ Dockerizing Django applications with production WSGI servers (gunicorn)
- ✅ Kubernetes workload debugging (CrashLoopBackOff, ImagePullBackOff)
- ✅ Service discovery and ClusterIP networking
- ✅ Ingress routing and custom domain mapping
- ✅ ConfigMap and Secret management
- ✅ Helm chart creation and templating
- ✅ CI/CD pipeline automation with GitHub Actions
- ✅ Docker image lifecycle and registry management
- ✅ Environment variable handling in containers
- ✅ Production-style architecture decisions

---

## 📚 Useful Commands

### Helm Management

```bash
# Install/upgrade release
helm upgrade --install django-app ./helm/django-app

# Uninstall release
helm uninstall django-app
# Template rendering (dry-run)
helm template django-app ./helm/django-app --debug

# List releases
helm list
```

### Prometheus & Grafana
```bash
# Port-forward Prometheus
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090

# Port-forward Grafana
kubectl port-forward svc/monitoring-grafana 3000:80

# Get Grafana password
kubectl get secret monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 --decode

# Check ServiceMonitor
kubectl get servicemonitor django-monitor -o yaml

# View Prometheus targets
# Open: http://localhost:9090/targets
```

### Kubernetes Debugging

```bash
# Watch pods
kubectl get pods -w

# View logs
kubectl logs -f deployment/django-app

# Describe pod (events, errors)
kubectl describe pod <pod-name>

# Port forwarding
kubectl port-forward service/django-app 8000:80

# Exec into container
kubectl exec -it deployment/django-app -- bash

# Restart deployment
kubectl rollout restart deployment/django-app
```

### PostgreSQL Management

```bash
# Access PostgreSQL
kubectl exec -it postgres-<pod-id> -- psql -U admin -d tasksdb

# Create database manually
kubectl exec -it postgres-<pod-id> -- psql -U admin -d postgres -c "CREATE DATABASE tasksdb;"
```

---

## 🤝 Contributing

- This is a personal learning project, but feel free to fork and experiment!

---

## 📧 Contact

-For questions or suggestions, please open an issue on GitHub.

---

### ⭐ Star this repo if you found it helpful!
