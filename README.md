# SD5818_MSA - Microservices Application

A full-stack microservices application with React frontend, Node.js/Express backend, and MongoDB database. The application is containerized with Docker and can be deployed to Kubernetes clusters on AWS EKS or Azure AKS.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Deployment](#deployment)
- [CI/CD Pipelines](#cicd-pipelines)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Contributing](#contributing)

## 🎯 Overview

This is a Todo application built with a microservices architecture, demonstrating modern cloud-native practices including:

- Containerization with Docker
- Orchestration with Kubernetes
- Multi-cloud deployment (AWS & Azure)
- Automated CI/CD pipelines
- GitOps with ArgoCD
- Infrastructure as Code

## 🏗️ Architecture

The application consists of three main services:

```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Frontend  │─────▶│   Backend   │─────▶│   MongoDB   │
│  (React)    │      │ (Node.js)   │      │  (Database) │
│  Port: 3000 │      │  Port: 3000 │      │  Port: 27017│
└─────────────┘      └─────────────┘      └─────────────┘
```

- **Frontend**: React application with Bootstrap UI
- **Backend**: Express.js REST API
- **Database**: MongoDB for data persistence

## 🛠️ Tech Stack

### Frontend
- React 17
- Bootstrap 5
- Axios
- Sass

### Backend
- Node.js
- Express.js
- Mongoose
- MongoDB Driver
- CORS
- Body Parser

### DevOps & Infrastructure
- Docker & Docker Compose
- Kubernetes (EKS, AKS)
- Jenkins
- Azure DevOps
- ArgoCD
- AWS ECR
- Azure ACR

## 📦 Prerequisites

- **Node.js** (v14 or higher)
- **Docker** & **Docker Compose**
- **kubectl** (for Kubernetes deployments)
- **AWS CLI** (for AWS deployments)
- **Azure CLI** (for Azure deployments)
- **Git**

## 🚀 Getting Started

### Local Development with Docker Compose

1. **Clone the repository**
   ```bash
   git clone https://github.com/hung-nashtech/SD5818_MSA.git
   cd SD5818_MSA
   ```

2. **Navigate to the source directory**
   ```bash
   cd src
   ```

3. **Build and start the services**
   ```bash
   docker-compose -f compose.yaml up --build
   ```

4. **Access the application**
   - Frontend: http://localhost:3000
   - Backend API: http://backend:3000

### Local Development without Docker

#### Backend Setup

1. Navigate to backend directory
   ```bash
   cd src/backend
   ```

2. Install dependencies
   ```bash
   npm install
   ```

3. Start MongoDB (ensure MongoDB is running locally)

4. Start the backend server
   ```bash
   npm start
   # or for development with auto-reload
   npm run dev
   ```

#### Frontend Setup

1. Navigate to frontend directory
   ```bash
   cd src/frontend
   ```

2. Install dependencies
   ```bash
   npm install
   ```

3. Start the development server
   ```bash
   npm start
   ```

4. Access the application at http://localhost:3000

## 🌐 Deployment

### AWS EKS Deployment

The application can be deployed to AWS EKS using the manifests in `Manifest-AWS/`:

```bash
# Apply MongoDB
kubectl apply -f Manifest-AWS/mongodb.yaml

# Apply Backend
kubectl apply -f Manifest-AWS/backend.yaml

# Apply Frontend
kubectl apply -f Manifest-AWS/frontend.yaml

# Apply Ingress (if configured)
kubectl apply -f Manifest-AWS/ingress.yml
```

**AWS Configuration:**
- Region: `ap-southeast-1`
- ECR Registry: `811492260998.dkr.ecr.ap-southeast-1.amazonaws.com`
- Frontend Image: `bndz/frontend`
- Backend Image: `bndz/backend`

### Azure AKS Deployment

The application can be deployed to Azure AKS using the manifests in `Manifest-Azure/`:

```bash
# Apply MongoDB
kubectl apply -f Manifest-Azure/mongodb.yaml

# Apply Backend
kubectl apply -f Manifest-Azure/backend.yaml

# Apply Frontend
kubectl apply -f Manifest-Azure/frontend.yaml

# Apply Ingress (if configured)
kubectl apply -f Manifest-Azure/ingress.yml
```

**Azure Configuration:**
- Container Registry: `bndzacr.azurecr.io`

### ArgoCD GitOps Deployment

The application is configured for GitOps deployment using ArgoCD:

```bash
# Apply ArgoCD Application manifest
kubectl apply -f applicationargo.yaml -n argocd
```

ArgoCD will automatically sync and deploy changes from the configured Git repository.

## 🔄 CI/CD Pipelines

### Jenkins Pipeline

The Jenkins pipeline (`Jenkinsfile`) automates:

1. **Checkout** - Retrieves code from repository
2. **Setup** - Configures AWS CLI and kubectl
3. **Build & Push** - Builds Docker images and pushes to AWS ECR
4. **Deploy** - Deploys to EKS cluster

**Trigger**: Automatic on code commit

**Environment Variables**:
- `AWS_REGION`: ap-southeast-1
- `AWS_ACCOUNT_ID`: 811492260998
- `EKS_CLUSTER_NAME`: my-eks-cluster

### Azure DevOps Pipeline

The Azure pipeline (`azure-pipelines.yml`) automates:

1. **BuildAndPush Stage**:
   - Builds frontend and backend Docker images
   - Pushes to Azure Container Registry

2. **Deploy Stage**:
   - Deploys MongoDB, Backend, and Frontend to AKS

**Trigger**: Commits to `main` branch

**Agent Pool**: BNDZ

## 📁 Project Structure

```
SD5818_MSA/
├── applicationargo.yaml          # ArgoCD application configuration
├── azure-pipelines.yml           # Azure DevOps pipeline
├── Jenkinsfile                   # Jenkins pipeline
├── README.md                     # This file
├── documents/                    # Documentation
├── Manifest-AWS/                 # Kubernetes manifests for AWS
│   ├── backend.yaml
│   ├── frontend.yaml
│   ├── ingress.yml
│   └── mongodb.yaml
├── Manifest-Azure/               # Kubernetes manifests for Azure
│   ├── backend.yaml
│   ├── frontend.yaml
│   ├── ingress.yml
│   └── mongodb.yaml
└── src/
    ├── compose.yaml              # Docker Compose configuration
    ├── backend/                  # Backend service
    │   ├── Dockerfile
    │   ├── package.json
    │   ├── server.js
    │   ├── config/               # Configuration files
    │   ├── db/                   # Database connection
    │   ├── models/               # Data models
    │   ├── routes/               # API routes
    │   └── utils/                # Utility functions
    ├── frontend/                 # Frontend service
    │   ├── Dockerfile
    │   ├── package.json
    │   ├── public/
    │   └── src/
    │       ├── App.js
    │       ├── components/       # React components
    │       │   ├── AddTodo.js
    │       │   └── TodoList.js
    │       └── ...
    └── data/                     # MongoDB data directory
```

## ⚙️ Configuration

### Backend Configuration

Backend configuration is managed in `src/backend/config/`:
- `config.js` - Main configuration
- `config.json` - Environment-specific settings
- `messages.js` - Application messages

### Frontend Configuration

Frontend proxy configuration in `package.json`:
```json
"proxy": "http://backend:3000"
```

### Environment Variables

Create a `.env` file in the backend directory for local development:
```
MONGODB_URI=mongodb://mongo:27017/tododb
PORT=3000
NODE_ENV=development
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the ISC License.

## 👥 Authors

- **Syed Afzal** - Initial backend work
- **Hung Tran** - DevOps and Infrastructure

## 🔗 Related Resources

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
- [AWS EKS Documentation](https://docs.aws.amazon.com/eks/)
- [Azure AKS Documentation](https://docs.microsoft.com/en-us/azure/aks/)

---

**Repository**: [SD5818_MSA](https://github.com/hung-nashtech/SD5818_MSA)  
**Owner**: hung-nashtech  
**Branch**: main