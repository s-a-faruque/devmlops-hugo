+++
title = "Kubernetes Networking - Services, Ingress, and Network Policies"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "Networking", "Services", "Ingress", "Network Policies"]
draft = false
description = "Understand Kubernetes networking, including Services, Ingress, and Network Policies."
+++

Kubernetes networking is essential for communication between pods, services, and external clients. It enables pods to discover and communicate with each other dynamically while maintaining security and performance.

In this article, we will explore Kubernetes networking concepts, including **Services, Ingress, and Network Policies**, to help you manage traffic efficiently.

---

#### Kubernetes Networking Model

Kubernetes follows a flat networking model where:  
- Every pod gets its own **unique IP address**.  
- Pods in the same or different nodes can communicate **without NAT**.  
- Services provide **stable endpoints** for pod communication.  

To enable efficient communication, Kubernetes uses various networking components like **Services, Ingress, and Network Policies**.

---

#### Kubernetes Services

A Kubernetes **Service** provides a stable IP and DNS name for accessing a group of pods. It ensures reliable communication even if pod IPs change.

#### **Types of Services**  
1. **ClusterIP (Default)** – Exposes the service internally within the cluster.  
2. **NodePort** – Exposes the service externally on a static port of each node.  
3. **LoadBalancer** – Uses an external load balancer (cloud provider specific).  
4. **ExternalName** – Maps a service to an external domain.

#### Example: Creating a ClusterIP Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: default
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```
This exposes `my-app` internally on **port 80**, forwarding traffic to **port 8080** on matching pods.

---

#### Kubernetes Ingress

An **Ingress** is an API object that manages external access to services, usually via HTTP/HTTPS.

#### **Key Features of Ingress:
- Routes external requests to internal services.  
- Supports SSL/TLS termination.  
- Can perform path-based and host-based routing.

#### **Example: Creating an Ingress Resource**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /app
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```
This routes `example.com/app` requests to `my-service` inside the cluster.

---

#### Network Policies

**Network Policies** control how pods communicate with each other and external entities. They act as **firewall rules** for Kubernetes networking.

### **Example: Allowing Traffic Only from Specific Pods**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 80
```
This policy allows only **frontend** pods to access **web** pods on port **80**.

---

#### Best Practices for Kubernetes Networking

- **Use ClusterIP for internal services** to reduce exposure.  
- **Use Ingress with TLS encryption** for security.  
- **Implement Network Policies** to restrict unwanted communication.  
- **Monitor network traffic** using tools like `kubectl logs` and `kubectl port-forward`.  

---


Kubernetes networking is a crucial part of managing a cluster effectively. By understanding **Services, Ingress, and Network Policies**, you can build secure, scalable, and reliable applications.  


