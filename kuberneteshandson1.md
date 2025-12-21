# Kubernetes Beginner Documentation  
## Deploying a Dockerized React App using Minikube

---

## 1. Project Overview

In this project, I deployed a Dockerized React application on a local Kubernetes cluster using **Minikube**.  
The goal was to understand the **basic Kubernetes workflow from scratch**, starting from a Docker image to running the application inside a Kubernetes Pod and accessing it in the browser.

---

## 2. Prerequisites

Before starting Kubernetes deployment, the following tools were set up on my local machine:

- **Docker Desktop** – to build and run containers  
- **Docker Hub account** – to store the application image  
- **kubectl** – Kubernetes command-line tool  
- **Minikube** – to run a local Kubernetes cluster  
- **VS Code** – for writing YAML files  

---

## 3. Dockerizing the Application

The React application was first converted into a Docker image.

### Steps:
1. Created a `Dockerfile` for the React app  
2. Built the Docker image locally  
3. Pushed the image to Docker Hub  

### Docker Image Used:
```text
akshithauser/ecocycle-react:latest
```
This image is later pulled by Kubernetes when creating the pod.

---

## 4. Installing kubectl

kubectl is the command-line tool used to interact with Kubernetes clusters.

Verification:
kubectl version --client


This confirms that kubectl is installed correctly.

---

## 5. Installing and Starting Minikube

Minikube is used to run Kubernetes locally.

Start Minikube:
minikube start --driver=docker

Verify Cluster:
kubectl get nodes


Expected Output:

One node named minikube

Status: Ready

This confirms the Kubernetes cluster is running successfully.

---

## 6. Creating the Pod Configuration (pod.yaml)

A Kubernetes Pod configuration file was created using VS Code.
```
File: pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: ecocycle
spec:
  containers:
    - name: ecocycle-container
      image: akshithauser/ecocycle-react:latest
      ports:
        - containerPort: 80
```
### Explanation:

Creates a Pod named ecocycle

Runs one container inside the pod

Pulls the Docker image from Docker Hub

Exposes port 80 inside the container

---

## 7. Creating the Pod in Kubernetes

The pod was created using:

kubectl apply -f pod.yaml

Check Pod Status:
kubectl get pods


Expected Status:

Running


This confirms that the application container is running successfully.

---

## 8. Accessing the Application

Pods are not directly accessible from the browser.

To access the application locally, port forwarding was used.

Port Forward Command:
kubectl port-forward pod/ecocycle 8080:80


This maps:

Local port 8080

To container port 80

Application URL:
http://localhost:8080


The application loads successfully in the browser.

---

## 9. Key Learnings

Kubernetes runs applications inside Pods

Pods pull images from Docker Hub

kubectl is used to manage Kubernetes resources

Minikube enables running Kubernetes locally

Port forwarding allows local access without Services

Pods are suitable for learning but not for production
