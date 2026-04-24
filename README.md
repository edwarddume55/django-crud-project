# Django CRUD Platform (Day 1 - Docker Setup)

## 📌 Project Overview

This project is a simple **Django REST Framework CRUD API** for a task management system.  
It is containerized using **Docker and Docker Compose** with a **PostgreSQL database**.

This is the first step in building a full **DevOps/SRE portfolio project**, which will later include CI/CD, monitoring, and Kubernetes deployment.

---

## 🏗️ Tech Stack

- Django 5.1+
- Django REST Framework
- PostgreSQL
- Docker
- Docker Compose
- Python 3.11

---

## 📁 Project Structure

django-crud-platform/
├── config/
├── tasks/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── manage.py
└── .env (not committed)


---

## 🚀 Features (Day 1)

- Django project setup
- REST API for tasks
- PostgreSQL database integration
- Dockerized application
- Docker Compose orchestration
- Automatic database migrations on startup

---

## ⚙️ Setup Instructions

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
## 🌐 API Endpoint
| Method | Endpoint          | Description        |
|--------|------------------|--------------------|
| GET    | /api/tasks/      | List all tasks     |
| POST   | /api/tasks/      | Create a task      |
| PUT    | /api/tasks/<id>/ | Update a task      |
| DELETE | /api/tasks/<id>/ | Delete a task      |

---
## 📦 Current Status (Day 1)
- Django project created
- REST API implemented
- PostgreSQL configured
- Dockerized setup
- Docker Compose working
- Migrations running automatically

---
### 🔜 Next Steps (Day 2+)
- Kubernetes deployment (Minikube/K3s)
- CI/CD pipeline (GitHub Actions)
- Helm charts
- Monitoring (Prometheus + Grafana)
- Logging (Loki)
- Production hardening

---
---

## 🚀 Goal

This project evolves into a full production-grade DevOps system using Kubernetes, CI/CD pipelines, and observability tools.