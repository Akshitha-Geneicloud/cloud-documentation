# Kubernetes Deployment Documentation  
## Deploying a Dockerized React Application using Minikube

---

## 1. Introduction

This document explains **step-by-step** how a Dockerized React application was deployed on **Kubernetes** using **Minikube** on a Windows machine.

The goal of this setup was to:
- Learn Kubernetes from scratch
- Deploy an application using real Kubernetes components
- Understand Deployments, Services, and Autoscaling
- Access the application through Kubernetes networking

This documentation is written for **future reference** and **team explanation**.

---

## 2. Tools & Technologies Used

| Tool | Purpose |
|----|----|
| Docker | Containerize the React application |
| Docker Hub | Store the Docker image |
| Kubernetes | Container orchestration |
| Minikube | Local Kubernetes cluster |
| kubectl | CLI to interact with Kubernetes |
| VS Code | Writing YAML configuration files |
| Windows PowerShell | Command-line interface |

---

## 3. High-Level Architecture

User (Browser)
↓
Kubernetes Service (NodePort)
↓
Deployment
↓
Multiple Pods (Containers)
↓
Docker Image from Docker Hub

---

## 4. Dockerizing the Application

The React application was first converted into a Docker image.

### Steps Performed
1. Created a `Dockerfile`
2. Built the Docker image locally
3. Pushed the image to Docker Hub

### Docker Image Used
```text
akshithauser/ecocycle-react:latest
```
This image is pulled by Kubernetes while creating pods.

---

## 5. Setting Up Kubernetes Locally (Minikube)
### 5.1 Installing kubectl

kubectl is the command-line tool used to manage Kubernetes clusters.

Verification:

kubectl version --client

### 5.2 Starting Minikube

Minikube was used to create a local Kubernetes cluster.

minikube start --driver=docker


Cluster verification:

kubectl get nodes


Output confirmed:

Node name: minikube

Status: Ready

---

## 6. Kubernetes Folder Structure

All Kubernetes configuration files were kept inside a dedicated folder.

Ecocycle/
│
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── hpa.yaml
│
├── Dockerfile
├── src/
└── README.md


This is a best practice and keeps Kubernetes configs organized.

---

## 7. Deployment Configuration (deployment.yaml)

A Deployment ensures:

Multiple pod replicas

Self-healing

Support for autoscaling
```
deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ecocycle-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: ecocycle
  template:
    metadata:
      labels:
        app: ecocycle
    spec:
      containers:
        - name: ecocycle-container
          image: akshithauser/ecocycle-react:latest
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"
            limits:
              cpu: "500m"
```
Explanation

replicas: 2 → runs two pods

containerPort: 80 → app runs on port 80

resources → required for autoscaling

---

8. Service Configuration (service.yaml)

A Service exposes the application and balances traffic between pods.
```
service.yaml
apiVersion: v1
kind: Service
metadata:
  name: ecocycle-service
spec:
  type: NodePort
  selector:
    app: ecocycle
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```
Explanation

NodePort exposes the app externally

Routes traffic to all running pods

Allows browser access

---

9. Autoscaling Configuration (hpa.yaml)

The Horizontal Pod Autoscaler (HPA) automatically scales pods based on CPU usage.
```
hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: ecocycle-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ecocycle-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```
Explanation

Minimum pods: 2

Maximum pods: 5

Scales when CPU usage exceeds 50%

---

10. Applying Kubernetes Resources

All Kubernetes resources were created using:

kubectl apply -f k8s/


Verification:

kubectl get deployment
kubectl get pods
kubectl get svc
kubectl get hpa

---

11. Enabling Metrics Server (Required for Autoscaling)
```minikube addons enable metrics-server```


This enables CPU metrics collection for HPA.

---

12. Accessing the Application

The application was accessed using:

```minikube service ecocycle-service```


This command:

Finds the NodePort

Creates a tunnel

Opens the app in the browser automatically

---
# This the command used to access the web Application:
<img width="1920" height="1128" alt="Screenshot 2025-12-22 150745" src="https://github.com/user-attachments/assets/6a2f5403-2944-4963-b3fe-a4cfb5e6574d" />

# This is the web application:
<img width="1920" height="1128" alt="Screenshot 2025-12-22 150754" src="https://github.com/user-attachments/assets/4ad403e1-8e4e-4d67-8048-c6b83b0cac31" />
