+++
title = "Kubernetes Storage - Persistent Volumes and CSI"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "Storage", "Persistent Volumes", "CSI"]
draft = false
description = "Learn how Kubernetes manages storage using Persistent Volumes and the Container Storage Interface (CSI)."
+++

### **Introduction**  
Storage is an essential part of running stateful applications in Kubernetes. Unlike stateless applications, databases and other persistent workloads require reliable and scalable storage solutions.

In this article, we’ll explore **Persistent Volumes (PVs), Persistent Volume Claims (PVCs), Storage Classes, and the Container Storage Interface (CSI)**.

---

## **1. Understanding Kubernetes Storage**  

Kubernetes provides two primary types of storage:  

- **Ephemeral Storage**: Data is lost when a pod is deleted. Examples include `emptyDir`, `ConfigMap`, and `Secrets`.  
- **Persistent Storage**: Data survives even if pods restart or are rescheduled. Examples include **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)**.

---

## **2. Persistent Volumes (PV) and Persistent Volume Claims (PVC)**  

A **Persistent Volume (PV)** is a cluster-wide storage resource, while a **Persistent Volume Claim (PVC)** is a request for storage by a pod.

### **Example: Defining a Persistent Volume (PV)**
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-storage-class
  hostPath:
    path: "/mnt/data"
```
This PV provides **10Gi** of storage, accessible by a single pod at a time.

---

### **Example: Creating a Persistent Volume Claim (PVC)**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: my-storage-class
```
A pod can now claim this storage using the PVC.

---

### **Example: Using a PVC in a Pod**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  volumes:
    - name: my-storage
      persistentVolumeClaim:
        claimName: my-pvc
  containers:
    - name: my-container
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: my-storage
```
This pod mounts the **Persistent Volume** at `/usr/share/nginx/html`.

---

## **3. Storage Classes for Dynamic Provisioning**  

A **StorageClass** defines how storage is provisioned dynamically, removing the need to manually create Persistent Volumes.

### **Example: Creating a StorageClass**
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```
Now, any PVC requesting `fast-storage` will automatically provision an AWS EBS volume.

---

## **4. Container Storage Interface (CSI)**  

The **Container Storage Interface (CSI)** is a Kubernetes standard that allows external storage providers (AWS EBS, Google Persistent Disk, Ceph, etc.) to integrate with Kubernetes.

### **Key Benefits of CSI**:
- Works with **cloud and on-prem** storage solutions.  
- Enables **dynamic volume provisioning**.  
- Provides **consistent APIs** across storage providers.  

### **Example: Using a CSI Volume**
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: csi-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: csi-sc
  csi:
    driver: ebs.csi.aws.com
    volumeHandle: vol-1234567890abcdef0
```
This PV uses the **AWS EBS CSI driver**.

---

## **5. Best Practices for Kubernetes Storage**  

- **Use Storage Classes** to enable dynamic provisioning.  
- **Choose the right access mode** (`ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`).  
- **Monitor storage usage** with tools like `kubectl describe pvc` and `kubectl get pv`.  
- **Use CSI drivers** for cloud storage providers instead of `hostPath`.  

---

Kubernetes provides powerful storage solutions through **Persistent Volumes, Storage Classes, and the CSI interface**. By leveraging these, you can efficiently manage stateful applications with persistent data needs.

