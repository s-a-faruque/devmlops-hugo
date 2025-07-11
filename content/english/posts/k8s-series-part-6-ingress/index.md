+++
title = "Kubernetes Networking and Ingress Controllers Explained"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "K8s", "Networking", "Ingress", "Service"]
draft = false
description = "Learn how networking works in Kubernetes, including Services, Ingress Controllers, and how to expose applications."
+++

Kubernetes networking enables communication between pods, services, and external clients. It ensures connectivity, scalability, and security for applications running inside a cluster.

In this article, we will cover:
1. Pod-to-Pod communication
2. Services for exposing applications
3. Ingress Controllers for managing external access
<!--more-->

#### 1. How Kubernetes Networking Works

Unlike traditional networks, Kubernetes follows a flat networking model, where:
* Every pod gets a unique IP address.
* Pods can communicate with each other without NAT (Network Address Translation).
* Services provide a stable endpoint to expose pods to internal or external users.

Kubernetes networking is designed to be highly scalable and works across different cloud providers and on-premise clusters.

#### 2. Kubernetes Service Types

A Service is an abstraction that provides network access to a group of pods. Since pod IPs are dynamic, a Service ensures consistent connectivity.

Types of Services in Kubernetes

Service Type	Description
ClusterIP	Default type, accessible only within the cluster.
NodePort	Exposes the service on a static port on each node.
LoadBalancer	Uses a cloud provider’s load balancer to expose the service.
ExternalName	Maps a service to an external DNS name.

#### 3. Exposing an Application Using a Service

Example: Creating a ClusterIP Service
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
Apply the Service:
```
kubectl apply -f service.yaml
```
Check the Service:
```
kubectl get services
```
Pods can now access this service using my-service:80.

Example: Exposing an Application Using a NodePort
```
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30080  # Exposes the service on <NodeIP>:30080
```
Apply the Service:
```
kubectl apply -f nodeport-service.yaml
```
Access the application:
```
http://<NodeIP>:30080
```
Find the Node IP:
```
kubectl get nodes -o wide
```
4. Ingress Controller: Managing External Access

While NodePort and LoadBalancer expose services, Ingress Controllers offer a more flexible way to manage external traffic.

Ingress Controllers provide:
* Routing based on hostnames and paths.
* SSL/TLS termination for secure connections.
* Load balancing between multiple services.

5. Deploying an Ingress Controller

Example: Installing Nginx Ingress Controller

For Minikube:
```
minikube addons enable ingress
```
For other clusters using Helm:
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install my-ingress ingress-nginx/ingress-nginx
```
6. Configuring an Ingress Resource

Example: Ingress YAML for Two Services
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: myapp.local
      http:
        paths:
          - path: /service1
            pathType: Prefix
            backend:
              service:
                name: service1
                port:
                  number: 80
          - path: /service2
            pathType: Prefix
            backend:
              service:
                name: service2
                port:
                  number: 80
```
Apply the Ingress:
```
kubectl apply -f ingress.yaml
```
For local testing, add this to /etc/hosts:
```
127.0.0.1 myapp.local
```
Access the services:
```
http://myapp.local/service1
http://myapp.local/service2
```
7. Best Practices for Kubernetes Networking

* Use Services to expose applications internally.
* Use Ingress Controllers for external access and routing.
* Secure Ingress with TLS certificates for HTTPS.
* Implement Network Policies to restrict traffic flow.
* Monitor networking performance using Kubernetes metrics and logs.



Kubernetes networking allows efficient communication between pods, services, and external users. Understanding Services, Ingress, and Networking Policies is essential for deploying production-ready applications.

