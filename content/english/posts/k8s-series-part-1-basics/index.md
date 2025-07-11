+++
title = "Understanding Kubernetes Architecture"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "K8s", "Architecture"]
draft = false
description = "A deep dive into Kubernetes architecture, including Master and Worker nodes, API Server, Scheduler, and more."
+++

Kubernetes is a powerful container orchestration system that automates the deployment, scaling, and management of containerized applications. To fully understand how Kubernetes operates, we must first explore its architecture and components.

## High-Level Overview
A Kubernetes cluster consists of two main components:
* Control Plane (Master Node): Manages the cluster and ensures the desired state of applications.
* Worker Nodes: Execute containerized applications and respond to instructions from the control plane.

![K8s Architecture](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

### Kubernetes Control Plane Components
The Control Plane is responsible for managing cluster operations and consists of the following components:
1. API Server
	* Acts as the front-end of the Kubernetes cluster.
	* All communication happens via REST API requests.
	* kubectl and external clients interact with it.
2. Controller Manager
	* Ensures the desired state of the cluster.
3. Scheduler
	* Assigns new pods to the appropriate worker nodes based on resource availability and constraints.
4. etcd (Key-Value Store)
	* Stores all cluster data and configuration states.
	* Highly available and consistent across nodes.

### Kubernetes Worker Node Components
Each Worker Node runs application workloads and contains these essential components:
1. Kubelet
	* The primary agent on each worker node.
	* Communicates with the API server to execute containerized workloads.
2. Kube-Proxy
	* Manages network rules and ensures communication between pods and services.
3. Container Runtime 
    * Executes containers. 
    Common runtimes include - Docker, containerd, CRI-O
### Kubernetes Networking
* Each pod gets a unique IP address.
* Uses CNI (Container Network Interface) plugins such as Calico, Flannel, or Cilium.
* Services allow communication between pods, nodes, and external systems.

Understanding Kubernetes architecture is fundamental to managing and troubleshooting clusters. The control plane ensures desired state management, while worker nodes execute workloads.
