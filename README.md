# CampusFit Application

A Node.js student registration application for learning Docker and Kubernetes.

## 🚀 Quick Start

### 1. Setup

```bash
# Install dependencies
npm install

# Configure environment
cp .env.example .env
```

### 2. Start MongoDB

**Create MongoDB user:**
```bash
mongosh << 'EOF'
use admin
db.createUser({
  user: "campusfit_user",
  pwd: "campusfit_password",
  roles: [{ role: "readWrite", db: "campusfit_db" }]
})
exit
EOF
```

### 3. Run Application

```bash
npm start
```

Expected output:
```
✅ MongoDB connected successfully
🚀 CampusFit Application Started
📍 Server running on port 3000
```

## 📡 API Endpoints

### Health Check
```bash
curl http://localhost:3000/health
```

### Register Student
```bash
curl -X POST http://localhost:3000/student \
  -H "Content-Type: application/json" \
  -d '{"name":"Rahul","membership":"Premium"}'
```

**Membership types:** `Basic`, `Premium`, `Elite`

## 🧪 Test Everything

```bash
chmod +x test-api.sh
./test-api.sh
```

## 📦 What's Next?

This application runs locally. Your assignment is to **containerize it using Docker**.

You will:
- Write a `Dockerfile`
- Build and run Docker images
- Fix the MongoDB networking issue (hint: `localhost` won't work in containers!)
- Create `docker-compose.yml`
- Configure volumes for data persistence
- Push your image to Docker Hub

**The application is intentionally NOT Dockerized - that's your job!**

## 🐛 Troubleshooting

**MongoDB connection failed?**
```bash
sudo systemctl start mongod
mongosh --eval "db.version()"  # verify MongoDB is running
```

**Port already in use?**
```bash
lsof -ti:3000 | xargs kill -9
```

## ☸️ Kubernetes Implementation

### Beyond the Original Assignment

After successfully containerizing the application with Docker and Docker Compose, the application was deployed on Kubernetes using Minikube to demonstrate container orchestration, scaling, configuration management, persistence, and self-healing capabilities.

### Kubernetes Components Implemented

* Namespace
* Deployment
* ReplicaSet
* Pods
* Services (NodePort & ClusterIP)
* ConfigMaps
* Secrets
* Health Probes (Liveness & Readiness)
* Persistent Volumes (PV)
* Persistent Volume Claims (PVC)
* Resource Requests & Limits

### Kubernetes Project Structure

```text
k8s/
├── namespace.yaml
├── app-deployment.yaml
├── app-service.yaml
├── configmap.yaml
├── mongo-deployment.yaml
├── mongo-service.yaml
├── mongo-pvc.yaml
├── secret-example.yaml
└── screenshots/
```

### Docker Hub Image

```text
siddhesh0710/campusfit-app:v1
```

### Deploy on Kubernetes

```bash
kubectl apply -f k8s/namespace.yaml

kubectl apply -f k8s/mongo-pvc.yaml
kubectl apply -f k8s/mongo-deployment.yaml
kubectl apply -f k8s/mongo-service.yaml

kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml

kubectl apply -f k8s/app-deployment.yaml
kubectl apply -f k8s/app-service.yaml
```

### Verify Deployment

```bash
kubectl get all
kubectl get pvc
kubectl get configmap
kubectl get secrets
```

### Access the Application

```bash
minikube service campusfit-service
```

### Kubernetes Architecture

```text
Browser
    ↓
CampusFit Service (NodePort)
    ↓
CampusFit Pods (2 Replicas)
    ↓
MongoDB Service (ClusterIP)
    ↓
MongoDB Pod
    ↓
Persistent Volume Claim (PVC)
    ↓
Persistent Volume (PV)
```

### Features Demonstrated

* Docker containerization
* Docker Compose orchestration
* Docker Hub image publishing
* Kubernetes deployments
* Service-to-Service communication
* Configuration management using ConfigMaps
* Secret management using Kubernetes Secrets
* Persistent database storage using PV/PVC
* Self-healing Pods using Deployments and ReplicaSets
* Horizontal scaling through replica management
* Health monitoring using Liveness and Readiness Probes
* Resource management using CPU and Memory Requests/Limits
* Namespace-based resource isolation

### Documentation & Screenshots

Additional project documentation can be found in:

```text
docs/kubernetes-report.md
```

Deployment screenshots are available in:

```text
k8s/screenshots/
```


---

Good luck! 🚀
