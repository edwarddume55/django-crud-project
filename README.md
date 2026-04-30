# Django CRUD Platform (DevOps/SRE Project)

## 📌 Project Overview

This is a production-style Django REST API for a task management system, built as part of a DevOps/SRE learning journey.

The project has evolved beyond basic Docker setup into a Kubernetes-deployed microservice architecture with PostgreSQL, Ingress routing, Helm charts, and real-world debugging scenarios.

It will continue evolving into a full production-grade platform with CI/CD, monitoring, and observability.

---

## 🏗️ Tech Stack

- Django 5.1+
- Django REST Framework
- PostgreSQL 15
- Docker & Docker Compose
- Kubernetes (Minikube)
- NGINX Ingress Controller
- Helm Charts
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
│   └── secret.yaml
├── helm/                   # Helm charts
│   └── django-app/
│       ├── templates/
│       │   ├── deployment.yaml
│       │   ├── service.yaml
│       │   ├── ingress.yaml
│       │   └── secret.yaml
│       ├── Chart.yaml
│       └── values.yaml
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

## 🟢 Containerization

- Dockerized Django application
- Docker Compose setup with PostgreSQL
- Environment variable configuration

## 🟢 Kubernetes (Production Simulation)

- Deployment with multiple replicas
- PostgreSQL as separate deployment
- Kubernetes Service (ClusterIP)
- ConfigMaps & Secrets management
- Readiness & Liveness Probes
- Ingress Controller (NGINX)
- Custom domain routing (django.local)
- Helm Chart packaging and management

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

### 3. Run with Docker

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

### 3. Deploy PostgreSQL manually (or let Helm manage it)

```bash
    # Manual PostgreSQL (for control)
    kubectl apply -f k8s/postgres-deployment.yaml
    kubectl apply -f k8s/postgres-service.yaml

```

### 4. Deploy Django app with Helm

```bash
    cd helm/django-app

    # Remove any existing release
    helm uninstall django-app --ignore-not-found=true

    # Install with Helm
    helm install django-app .

    # Verify deployment
    kubectl get pods
    kubectl get svc
    kubectl get ingress
```

### 5. Configure hosts file (Windows/macOS/Linux)

Add to /etc/hosts (Linux/macOS) or C:\Windows\System32\drivers\etc\hosts (Windows):

```bash
    192.168.49.2  django-app.local
```

Get Minikube IP if needed:

```bash
    minikube ip
```

---

### 6. Run database migrations

```bash
    kubectl apply -f k8s/migrate-job.yaml
```

### 7. Create superuser (optional)

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
```

Via Minikube Service

```bash
minikube service django-app
```

---

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

---

## 🔜 Next Steps

### 📊 Observability

- Prometheus (metrics)
- Grafana (dashboards)
- Loki (logs)
- Jaeger tracing

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
Observability (Coming)
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
