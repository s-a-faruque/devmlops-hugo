+++
title = "Kubernetes Volumes and Persistent Volumes Explained"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "K8s", "Storage", "Volumes", "Persistent Volumes"]
draft = true
description = "Learn how Kubernetes handles storage using Volumes, Persistent Volumes (PVs), and Persistent Volume Claims (PVCs)."
+++

Containers are ephemeral by nature, meaning data stored inside them disappears when a pod is restarted. To persist data in Kubernetes, we use Volumes and Persistent Volumes.

1. Kubernetes Volumes: Basics

A Volume in Kubernetes provides storage that can be shared across containers within a pod. Unlike a container’s filesystem, a Kubernetes volume exists as long as the pod exists.

Types of Kubernetes Volumes:

Volume Type	Description
emptyDir	Temporary storage that exists while the pod is running.
hostPath	Mounts a directory from the worker node’s filesystem.
configMap / secret	Mounts a ConfigMap or Secret as a file.
persistentVolumeClaim	Connects to a Persistent Volume (PV) for long-term storage.

2. Example: Using an emptyDir Volume

An emptyDir volume is created when the pod starts and deleted when the pod stops.

apiVersion: v1
kind: Pod
metadata:
  name: emptydir-example
spec:
  containers:
    - name: app
      image: busybox
      command: ["sleep", "3600"]
      volumeMounts:
        - mountPath: "/data"
          name: my-volume
  volumes:
    - name: my-volume
      emptyDir: {}

Apply the configuration:

kubectl apply -f emptydir-pod.yaml

3. Persistent Volumes (PVs) and Persistent Volume Claims (PVCs)

A Persistent Volume (PV) is a cluster-wide storage resource. A Persistent Volume Claim (PVC) requests storage from a PV.

Storage Classes:

Kubernetes supports dynamic storage provisioning using StorageClasses, which define how storage should be created.

Example: Persistent Volume (PV)

apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: "/mnt/data"

Apply the PV:

kubectl apply -f pv.yaml

Example: Persistent Volume Claim (PVC)

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

Apply the PVC:

kubectl apply -f pvc.yaml

Using a PVC in a Pod

apiVersion: v1
kind: Pod
metadata:
  name: pvc-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: "/data"
          name: storage
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: my-pvc

Apply the pod:

kubectl apply -f pvc-pod.yaml

Check the PVC status:

kubectl get pvc

4. Persistent Volume Reclaim Policies

Kubernetes provides three reclaim policies for Persistent Volumes:

Reclaim Policy	Description
Retain	Keeps the volume even after the PVC is deleted.
Recycle	Clears the volume but retains it for reuse. (Deprecated)
Delete	Deletes the storage when the PVC is removed (used for cloud-based storage).

5. Choosing the Right Storage Option

Storage Requirement	Recommended Volume Type
Temporary storage within a pod	emptyDir
Shared storage between containers	hostPath (not recommended for production)
Configurations or credentials	configMap or secret
Persistent storage for databases or logs	PersistentVolumeClaim

Kubernetes Volumes and Persistent Volumes allow applications to store and persist data beyond the lifecycle of a pod. Understanding when to use ephemeral volumes versus persistent storage is crucial for designing scalable applications in Kubernetes.
