
# Kubernetes Documentation 
### Overview of kubernetes documentation:
- Introduction to Kubernetes

- Why Kubernetes is Needed

- Kubernetes vs Docker

- Kubernetes Architecture

- Control Plane Components

- Worker Node Components

- Core Kubernetes Objects

- Kubernetes Application Workflow

- Scaling in Kubernetes

- Self-Healing Mechanism

- Rolling Updates and Rollbacks

- Kubernetes Networking Basics

- Kubernetes Storage

- Types of Kubernetes Clusters

- Kubernetes Orchestration Features

- Kubernetes Commands Cheat Sheet

- Kubernetes in the DevOps Ecosystem

- Real-World Use Case

- Summary

## What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform used to automate the **deployment, scaling, management, and networking of containerized applications**.

In simple words:

> Kubernetes manages containers (Docker) for you in production.

It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

---

## Why Kubernetes is Needed?

### Problems Without Kubernetes

* Manual container deployment
* Difficult scaling
* No self-healing
* No load balancing
* Downtime during updates

### Kubernetes Solves These By Providing:

* Automatic deployment & scaling
* Self-healing (restart failed containers)
* Load balancing
* Rolling updates & rollbacks
* Efficient resource utilization

---

## Kubernetes vs Docker

| Docker                   | Kubernetes                  |
| ------------------------ | --------------------------- |
| Builds & runs containers | Manages containers at scale |
| Single host focus        | Multi-node cluster          |
| No auto-scaling          | Auto-scaling                |
| No self-healing          | Self-healing                |

**Docker is used first, Kubernetes comes after**

---

## Kubernetes Architecture

A Kubernetes cluster consists of:

### 1. Control Plane (Master Node)

Manages the cluster

### 2. Worker Nodes

Runs application workloads

---

## Control Plane Components

### 1.API Server

* Entry point to the cluster
* Communicates via REST API
* kubectl talks to API Server

### 2.etcd

* Distributed key-value store
* Stores cluster state
* Brain of Kubernetes

### 3.Scheduler

* Assigns Pods to nodes
* Decides *where* workloads run

### 4.Controller Manager

* Ensures desired state
* Examples: Node controller, ReplicaSet controller

---

## Worker Node Components

### 1.Kubelet

* Agent running on each node
* Talks to API Server
* Ensures Pods are running

### 2.Container Runtime

* Runs containers
* Examples: Docker, containerd, CRI-O

### 3.Kube-Proxy

* Handles networking
* Enables service-to-pod communication

---

## Core Kubernetes Objects

### 1.Pod

* Smallest deployable unit
* Contains one or more containers

### 2.ReplicaSet

* Ensures a specified number of Pod replicas

### 3.Deployment

* Manages ReplicaSets
* Supports rolling updates & rollbacks

### 4.Service

* Exposes Pods
* Stable networking endpoint

Types of Services:

* ClusterIP (default)
* NodePort
* LoadBalancer

### 5.Namespace

* Logical isolation
* Used for environments (dev, prod)

---

## Kubernetes Workflow (Real Life)

1. Developer writes code
2. Build Docker image
3. Push image to registry (DockerHub/ECR)
4. Create Kubernetes YAML files
5. Apply YAML using kubectl
6. Kubernetes schedules Pods
7. Service exposes application
---

## Scaling in Kubernetes

### Manual Scaling

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

### Auto Scaling (HPA)

* Based on CPU/Memory
* Requires Metrics Server

---

## Self-Healing in Kubernetes

Kubernetes automatically:

* Restarts failed containers
* Reschedules Pods
* Replaces unhealthy Pods

---

## Rolling Updates & Rollbacks

### Rolling Update

* Updates Pods gradually
* Zero downtime

### Rollback

```bash
kubectl rollout undo deployment nginx-deployment
```

---

## Kubernetes Networking

* Each Pod gets unique IP
* Pods can talk to each other
* Services provide stable access

---

## Kubernetes Storage

### Volumes

* EmptyDir
* HostPath
* PersistentVolume (PV)
* PersistentVolumeClaim (PVC)

---

## Types of Kubernetes Clusters

### 1.Local Clusters

* Minikube
* Kind

### 2.Cloud Managed Clusters

* EKS (AWS)
* AKS (Azure)
* GKE (Google Cloud)
Managed Kubernetes Services: AKS, EKS & GKE
1. What Are Managed Kubernetes Services?

Managed Kubernetes services are cloud-provided platforms where the cloud provider manages the Kubernetes control plane (API Server, etcd, scheduler).
You focus on deploying and running applications, not maintaining Kubernetes itself.

Examples:

AKS – Azure Kubernetes Service (Microsoft Azure)

EKS – Elastic Kubernetes Service (AWS)

GKE – Google Kubernetes Engine (Google Cloud)

2. Azure Kubernetes Service (AKS)
What is AKS?

AKS is Microsoft Azure’s managed Kubernetes service that simplifies cluster creation, scaling, and upgrades.

What Azure Manages

Control Plane (API Server, etcd)

Automatic upgrades

High availability of master components

What You Manage

Worker nodes (VMs)

Applications & Kubernetes resources

Key Characteristics

Control plane is free

Easy integration with Azure services

Simple cluster setup

When to Use AKS

When your infrastructure is on Azure

When you want simple, low-cost Kubernetes

When you are new to Kubernetes

<img width="596" height="502" alt="Screen-Shot-2020-03-18-at-12 07 32-PM" src="https://github.com/user-attachments/assets/a8b09466-6175-429e-a207-8f7ec8bf083e" />

3. Amazon Elastic Kubernetes Service (EKS)
What is EKS?

EKS is AWS’s managed Kubernetes service designed for enterprise-grade, secure Kubernetes workloads.

What AWS Manages

Control Plane

etcd

Security patches and availability

What You Manage

Worker nodes (EC2 or Fargate)

Networking and scaling configurations

Applications

Key Characteristics

Deep AWS security integration (IAM)

Supports EC2 and Fargate (serverless pods)

Runs across multiple Availability Zones

When to Use EKS

When using AWS cloud services

When you need high security & compliance

When building large-scale production systems
<img width="1136" height="867" alt="1_s5NYOU2VDbu9YYU1OTI6Iw" src="https://github.com/user-attachments/assets/aec0345a-d57a-4f4d-9429-0bb500c7e442" />

4. Google Kubernetes Engine (GKE)
What is GKE?

GKE is Google’s managed Kubernetes service. Kubernetes was originally built by Google, making GKE the most Kubernetes-native platform.

What Google Manages

Control Plane

Node health and upgrades

Automatic scaling

What You Manage

Applications (even less with Autopilot mode)

Key Characteristics

Autopilot mode (Google manages nodes too)

Fastest Kubernetes version updates

Advanced networking

When to Use GKE

When Kubernetes is your core platform

When you want maximum automation

When you want minimal operational effort
<img width="819" height="499" alt="image1-8" src="https://github.com/user-attachments/assets/dfc5aa69-51aa-4e1b-89ab-cdb29fc43bf2" />

### 3.Self-Managed Clusters

* kubeadm
* Bare metal

---

## Kubernetes Orchestration Features

* Container scheduling
* Auto-scaling
* Self-healing
* Service discovery
* Load balancing
* Configuration management
* Secret management
---

## Kubernetes + DevOps Tools

* Docker → Containerization
* Git → Source control
* Jenkins/GitHub Actions → CI/CD
* Kubernetes → Orchestration
* Helm → Package manager
* Prometheus & Grafana → Monitoring
---

## Real-Time Use Case Example

* Microservices application
* CI/CD pipeline deploys Docker images
* Kubernetes manages scaling
* LoadBalancer exposes app
* Auto-healing ensures uptime

---

## Final Summary

Kubernetes is:

* A container orchestration tool
* Production-grade
* Cloud-native
* Essential for DevOps & Cloud roles

---
