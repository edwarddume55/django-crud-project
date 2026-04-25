# Django CRUD Platform (Day 1 - Docker Setup)

## 📌 Project Overview

This is a production-style Django REST API for a task management system, built as part of a DevOps/SRE learning journey.

The project has evolved beyond basic Docker setup into a Kubernetes-deployed microservice architecture with PostgreSQL, Ingress routing, and real-world debugging scenarios.

It will continue evolving into a full production-grade platform with CI/CD, monitoring, and observability.

---

## 🏗️ Tech Stack

- Django 5.1+
- Django REST Framework
- PostgreSQL
- Docker
- Docker Compose
- Kubernetes (Minikube)
- NGINX Ingress Controller
- Python 3.11

---

## 📁 Project Structure
```bash
django-crud-platform/
├── config/              
├── tasks/                  
├── k8s/                  
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   └── secret.yaml
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
- Deployment (Django app replicas)
- PostgreSQL Deployment
- Kubernetes Service (ClusterIP)
- ConfigMaps & Secrets
- Readiness & Liveness Probes
- Ingress Controller (NGINX)
- Custom domain routing (django.local)

## 🟢 DevOps Debugging Experience
- Fixed CrashLoopBackOff issues
- Resolved PostgreSQL connection misconfiguration
- Debugged environment variable conflicts
- Fixed Ingress 503 service routing issues
- Handled Kubernetes networking and service discovery

---

## ⚙️ Local Setup (Docker)

### 1. Clone repository

```bash
git clone https://github.com/your-username/django-crud-platform.git
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
---

### ☸️ Kubernetes Deployment (Minikube)
### 1. Start cluster
```bash
    minikube start --driver=docker
```
### 2. Enable Ingress
```bash
    minikube addons enable ingress
```
### 3. Apply manifests
```bash
    kubectl apply -f k8s/
```
### 4. Map local domain
```bash
    sudo nano /etc/hosts
```
Add:
```bash
    127.0.0.1 django.local
```
🌍 Access Application
```bash
    http://django.local/api/tasks/
```
--- 

## 🌐 API Endpoint

| Method | Endpoint          | Description       |
|--------|------------------|--------------------|
| GET    | /api/tasks/      | List all tasks     |
| POST   | /api/tasks/      | Create a task      |
| PUT    | /api/tasks/<id>/ | Update a task      |
| DELETE | /api/tasks/<id>/ | Delete a task      |
| GET    |  /health/        |   Health check     |


---
## 📦 Current Status
### ✅ Completed
- Django REST API
- Docker + Compose setup
- PostgreSQL integration
- Kubernetes deployment
- Service + ConfigMap + Secret
- Ingress routing
- Debugged real production-like failures

---

## 🔜 Next Steps

### 🚀 CI/CD Pipeline (GitHub Actions)
- Automated build
- Docker image push
- Kubernetes deployment automation

### 📊 Observability
- Prometheus (metrics)
- Grafana (dashboards)
- Loki (logs)

### ⚙️ Production Hardening
- HPA (autoscaling)
- Resource limits
- RBAC
- Helm charts

---

## 🚀 Goal

- This project demonstrates a real-world DevOps/SRE system lifecycle:

From application → containerization → Kubernetes → networking → CI/CD → observability → production readiness

---
### 🧠 Key Learning Outcomes
- Kubernetes workload debugging (CrashLoopBackOff, 503 errors)
- Service discovery and networking
- Ingress routing and domain mapping
- Environment variable management in clusters
- Production-style architecture thinking