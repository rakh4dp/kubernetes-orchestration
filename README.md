# Kubernetes Orchestration

A fullstack application (Node.js backend + React frontend) containerized with Docker and orchestrated using Kubernetes.

## Architecture

```
Browser
↓
frontend-service (:30080)
↓
React App (nginx)
↓
backend-service (:30011)
↓
Express API (Node.js)
```

## Project Structure

```
kubernetes-orchestration/
├── backend/
│   ├── Dockerfile
│   ├── server.js
│   └── package.json
├── frontend/
│   ├── Dockerfile
│   ├── public/
│   └── src/
│       └── App.js
└── klaster/
    ├── backend-deployment.yaml
    ├── backend-service.yaml
    ├── frontend-deployment.yaml
    └── frontend-service.yaml
```

## Prerequisites

- Docker Desktop (with Kubernetes enabled) or Minikube
- kubectl
- Node.js

Verify installations:

```bash
kubectl version --client
minikube version
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/rakh4dp/kubernetes-orchestration.git
cd kubernetes-orchestration
```

### 2. Build and push Docker images

Replace `<your-dockerhub-username>` with your Docker Hub username.

```bash
docker build -t <your-dockerhub-username>/backend:v1 ./backend
docker push <your-dockerhub-username>/backend:v1

docker build -t <your-dockerhub-username>/frontend:v1 ./frontend
docker push <your-dockerhub-username>/frontend:v1
```

> Update the `image` field in `klaster/backend-deployment.yaml` and `klaster/frontend-deployment.yaml` to match your Docker Hub image name.

### 3. Deploy to Kubernetes

```bash
kubectl apply -f klaster/backend-deployment.yaml
kubectl apply -f klaster/backend-service.yaml
kubectl apply -f klaster/frontend-deployment.yaml
kubectl apply -f klaster/frontend-service.yaml
```

### 4. Verify the cluster

```bash
kubectl get pods
kubectl get svc
```

## Access the Application

| Service  | URL                        |
|----------|----------------------------|
| Backend  | http://localhost:30011/api |
| Frontend | http://localhost:30080     |

## Port Configuration

| Service          | Type     | Port | Target Port | Node Port |
|------------------|----------|------|-------------|-----------|
| backend-service  | NodePort | 3011 | 3011        | 30011     |
| frontend-service | NodePort | 80   | 80          | 30080     |

## Tech Stack

- Node.js + Express
- React.js
- Docker
- Kubernetes