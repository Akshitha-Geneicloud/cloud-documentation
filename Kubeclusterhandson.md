# Kubernetes Cluster Setup on AWS EC2 using kubeadm

## Overview

This project documents the complete end-to-end setup of a **multi-node Kubernetes cluster** using **kubeadm** on **AWS EC2 Ubuntu instances**, including real-world issues faced during setup and how they were resolved. The cluster uses **containerd** as the container runtime and **Calico** as the CNI plugin.

The goal of this project was to gain hands-on experience with Kubernetes cluster creation, networking, node joining, and debugging production-like issues.

---

## Infrastructure Details

* **Cloud Provider:** AWS EC2
* **Operating System:** Ubuntu 22.04 LTS
* **Instance Type:** t3.small
* **Kubernetes Version:** v1.29.15
* **Container Runtime:** containerd
* **CNI Plugin:** Calico
* **Cluster Type:** kubeadm-based

### Nodes

| Role          | Private IP       |
| ------------- | ---------------- |
| Control Plane | ip-xxx-xx-xxx-xx |
| Worker Node   | ip-xx-xx-xxx-xxx |

---

## 🛠️ Step-by-Step Implementation

### 1️. EC2 Instance Setup

* Launched **two Ubuntu EC2 instances** in the same VPC and subnet
* Configured Security Group to allow:

  * SSH (22)
  * Kubernetes API Server (6443)
  * All traffic within VPC CIDR
 
 ### Why EC2?

    Kubernetes needs multiple machines to form a cluster

    EC2 gives:

    Private networking (VPC)

    Real server-like environment

    Control over firewall (Security Groups)

    Why Ubuntu 22.04?
    
    kubeadm officially supports Ubuntu very well
    
    Stable kernel + systemd
    
    Large community support
    
    Why 2 instances?
    
    Kubernetes is a distributed system
    
    You need:
    
    1 Control Plane → manages cluster
    
    1 Worker Node → runs applications

---

### 2️. System Preparation (Both Nodes)

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
```

Disable swap (required by Kubernetes):

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```
### Why Kubernetes disables swap:

    Kubernetes schedules pods based on actual memory
    
    Swap hides real memory usage
    
    Can cause:
    
    Pod eviction issues
    
    Unpredictable performance
    
    That’s why kubeadm enforces:
    swapoff -a
    This ensures deterministic memory management
---

### 3️. Install containerd (Both Nodes)

```bash
sudo apt install -y containerd
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```
### Why Kubernetes needs a container runtime:

    Kubernetes does not run containers itself
    
    It delegates to a runtime via CRI
    
    Why containerd:
    
    Lightweight
    
    CNCF-supported
    
    Default runtime for modern Kubernetes
    
    Docker is deprecated as a runtime
    
    - This aligns with production Kubernetes standards

**Critical Configuration Fix**
Edited `/etc/containerd/config.toml`:

```toml
SystemdCgroup = true
```

Restart services:

```bash
sudo systemctl restart containerd
sudo systemctl restart kubelet
```

---

### 4️. Install Kubernetes Components (Both Nodes)

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```
### kubeadm

- Used to bootstrap the cluster

- Creates certificates, configs, control-plane components

### kubelet

- Runs on every node

- Talks to API server

- Manages pods on the node

### kubectl

- CLI tool

- Used to interact with the cluster

-> Each has a distinct role, none are optional.
---

### 5️. Initialize Control Plane (Master Node)

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```
### Why this step:

- Initializes the control plane

- Creates:

- etcd

- API server

- scheduler

- controller manager
### Why --pod-network-cidr?

- Kubernetes does not create networking

- CNI plugin (Calico) needs a CIDR range

- Must match Calico’s default network

->  Without this, pods cannot communicate.
### Configure kubectl:

```bash
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
### Why this step:

- kubectl needs:

- API server address

- Certificates

- Authentication info

### Without this:

- kubectl tries to connect to:

localhost:8080


-That’s why I initially saw:

connection refused


-> This step links kubectl ↔ cluster securely
---

### 6️. Install Calico CNI (Master Node)

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml
```
### Why Kubernetes needs a CNI:

  - Kubernetes does NOT handle networking
  
  - Pods need:
  
  - IP addresses
  
  - Pod-to-pod communication
  
 - Cross-node networking

### Why Calico:

  - Stable
  
  - Production-grade
  
### Supports:
  
  - Network policies
  
  - Multi-node clusters
  
### What Calico provides:
  
  - Pod IP allocation
  
  - Routing between nodes
  
  - Network policy enforcement

-> Without CNI → nodes remain NotReady
---

### 7️. Join Worker Node

```bash
sudo kubeadm join <MASTER_PRIVATE_IP>:6443 \
--token <TOKEN> \
--discovery-token-ca-cert-hash sha256:<HASH>
```
### Why this step exists:

  - Securely adds worker nodes
  
  ### Uses:
    
    - Token
    
    - CA hash
    
    - API server endpoint

---

## Challenges Faced & Solutions

### Issue 1: kubeadm init failed due to low memory

**Error:**

```
the system RAM (914 MB) is less than the minimum 1700 MB
```

-> **Solution:**

* Changed EC2 instance type from `t3.micro` to `t3.small`

---

### Issue 2: Worker node unable to join cluster

**Error:**

```
Failed to connect to API server on port 6443
```

-> **Solution:**

* Fixed AWS Security Group inbound rules
* Allowed port `6443` within VPC CIDR

---

### Issue 3: Nodes stuck in NotReady state

**Symptoms:**

* `kubectl get nodes` → NotReady
* CoreDNS and Calico pods stuck in Pending

**Root Cause:**

* containerd was using `cgroupfs`
* Kubernetes expects `systemd` cgroups

-> **Solution:**

* Updated containerd config:

```toml
SystemdCgroup = true
```

* Restarted containerd and kubelet

---

### Issue 4: Calico & CoreDNS pods Pending

**Error:**

```
node(s) had untolerated taint node.kubernetes.io/not-ready
```

-> **Solution:**

* Fixed underlying node readiness issue
* Kubernetes automatically removed taints
* Pods transitioned to Running state

---

## Final Cluster Status

```bash
kubectl get nodes
```

```
NAME               STATUS   ROLES           VERSION
control-plane      Ready    control-plane   v1.29.15
worker-node        Ready    <none>          v1.29.15
```

```bash
kubectl get pods -n kube-system
```

All system pods are in **Running** state.

---

## Conclusion

This project provided deep hands-on experience with:

* kubeadm-based Kubernetes setup
* AWS EC2 networking
* Container runtime configuration
* CNI troubleshooting
* Real-world debugging of NotReady nodes

The cluster is now stable, fully networked, and ready for deploying applications, services, and autoscaling workloads.

---

## Next Enhancements

* Deploy sample applications
* Configure Services & Ingress
* Install Metrics Server
* Implement HPA (Horizontal Pod Autoscaler)
* Convert this setup into a reusable project template

---

*This documentation reflects a real-world Kubernetes setup and troubleshooting experience, not a simple tutorial deployment.*
