# Simple Application Deployment on Kubernetes (Without Autoscaling)

## 1. Overview

This document explains **everything I have done step by step** to containerize a simple application and deploy it on a Kubernetes cluster. It also explains **why each step was required**, the **commands used**, the **errors faced**, and **how those errors were resolved**.

The goal of this task was to understand the **end-to-end Kubernetes workflow**, not just theory.

---

## 2. Objective

* Build a simple application
* Containerize it using Docker
* Push the image to Docker Hub
* Deploy the application on Kubernetes
* Expose it and access it from a browser

Autoscaling was explored later but is **intentionally excluded** from this documentation.

---

## 3. Prerequisites

* Ubuntu EC2 instance
* Docker installed
* Kubernetes cluster already created (control plane + worker)
* kubectl configured
* Docker Hub account

---

## 4. Application Creation

### What I did

I created a **simple Python Flask application** that returns a response when accessed.

### Why this step

Before Kubernetes, we need **something to deploy**. A simple app helps focus on infrastructure rather than code complexity.

### Files created

* `app.py`
* `requirements.txt`

---

## 5. Dockerfile Creation

### What I did

I wrote a `Dockerfile` to package the application into a Docker image.

### Why this step

Kubernetes does **not run source code directly**. It runs **containers**, so the application must be containerized.

### Dockerfile (summary)

* Used `python:3.9-slim` as base image
* Set working directory
* Installed dependencies
* Copied application code
* Exposed port 5000
* Started the app using CMD

---

## 6. Building the Docker Image

### Command used

```bash
sudo docker build -t akshithauser/simple-k8s-app:v1 .
```

### Why this step

This converts the application into a **reusable container image**.

### Error faced

```text
permission denied while trying to connect to the Docker daemon socket
```

### Why this error occurred

The current user did not have permission to access Docker.

### How I solved it

Used `sudo` to run Docker commands.

---

## 7. Pushing Image to Docker Hub

### What I did

Pushed the image to Docker Hub so Kubernetes nodes can pull it.

### Command

```bash
sudo docker push akshithauser/simple-k8s-app:v1
```

### Why this step

Kubernetes pulls images from a **registry**, not local Docker.

---

## 8. Kubernetes Deployment

### What I did

Created a Deployment YAML to run the container as Pods.

### Why Deployment

* Ensures desired number of pods
* Restarts pods if they crash
* Makes the app scalable later

### Applied using

```bash
kubectl apply -f deployment.yaml
```

### Verified

```bash
kubectl get pods
```

Pods moved to **Running** state.

---

## 9. Exposing the Application

### What I did

Exposed the deployment using a Service.

### Why this step

Pods have **internal IPs**. A Service provides:

* Stable access
* External connectivity

### Command

```bash
kubectl apply -f service.yaml
```

### Checked service

```bash
kubectl get svc
```

---

## 10. Accessing the Application

### Initial issue

Browser showed:

```text
This site can’t be reached (ERR_CONNECTION_TIMED_OUT)
```

### Why this happened

* Security group did not allow traffic
* Port mapping confusion

### How I fixed it

* Verified NodePort
* Opened required port in EC2 security group
* Ensured application was listening on correct port

### Final result

The application became accessible from the browser.

---

## 11. Key Errors Faced & Learnings

### Docker permission error

* Learned about Docker daemon access

### Connection timeout

* Understood networking, NodePort, and security groups

### Kubernetes debugging

Used:

```bash
kubectl describe pod
kubectl logs <pod-name>
```

---

## 12. What I Achieved

* Built a containerized application
* Understood Docker → Kubernetes flow
* Deployed and exposed an app successfully
* Debugged real-world infrastructure issues

---

## 13. Conclusion

This task helped me understand **how applications actually reach users through Kubernetes**, not just YAML files. Each error improved my understanding of Docker, networking, and Kubernetes internals.

Autoscaling was explored separately and is not included here.

---
