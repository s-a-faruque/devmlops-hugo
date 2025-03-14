+++
title = "Kubernetes StatefulSets and DaemonSets Explained"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "K8s", "StatefulSets", "DaemonSets", "Workloads"]
draft = true
description = "Learn how Kubernetes manages stateful applications with StatefulSets and ensures node-specific workloads using DaemonSets."
+++

Introduction

Kubernetes workloads are typically managed using Deployments, which handle stateless applications. However, some applications require stable network identities or storage persistence, while others must run on every node in the cluster.

✅ StatefulSets are used for stateful applications like databases.
✅ DaemonSets are used for node-specific workloads like logging and monitoring agents.

In this article, we’ll explore both in detail.

1. Kubernetes StatefulSets

A StatefulSet is a Kubernetes resource designed for stateful applications where each pod:
✔️ Has a stable hostname and persistent storage.
✔️ Maintains ordering and identity when scaling or restarting.
✔️ Is created sequentially (not in parallel like Deployments).

Use Cases for StatefulSets

✅ Databases (e.g., MySQL, PostgreSQL, MongoDB).
✅ Distributed applications (e.g., Kafka, Elasticsearch, Zookeeper).
✅ Applications needing stable network identifiers.

2. Deploying a StatefulSet

Example: StatefulSet for a MySQL Database

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-storage
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi

Key Features in the Above StatefulSet:

✔️ Pods have stable hostnames (mysql-0, mysql-1, mysql-2).
✔️ Each pod gets a separate storage volume (volumeClaimTemplates).
✔️ Pods start one at a time in order (mysql-0 → mysql-1 → mysql-2).

3. Scaling a StatefulSet

kubectl scale statefulset mysql --replicas=5

This will add two more pods (mysql-3, mysql-4) in order.

4. Accessing StatefulSet Pods

Each pod in a StatefulSet can be accessed using:

mysql-0.mysql.default.svc.cluster.local
mysql-1.mysql.default.svc.cluster.local

where:
	•	mysql-0 is the pod name.
	•	mysql.default.svc.cluster.local is the headless service domain.

5. Kubernetes DaemonSets

A DaemonSet ensures that a specific pod runs on every node in the cluster. It automatically schedules pods on existing and new nodes.

Use Cases for DaemonSets

✅ Logging agents (e.g., Fluentd, Logstash).
✅ Monitoring agents (e.g., Prometheus Node Exporter).
✅ Networking components (e.g., CNI plugins).

6. Deploying a DaemonSet

Example: DaemonSet for a Node Logging Agent

apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      app: fluentd
  template:
    metadata:
      labels:
        app: fluentd
    spec:
      containers:
        - name: fluentd
          image: fluent/fluentd
          volumeMounts:
            - name: varlog
              mountPath: /var/log
      volumes:
        - name: varlog
          hostPath:
            path: /var/log

Key Features of DaemonSets:

✔️ Each node gets exactly one Fluentd pod.
✔️ Automatically adds pods when new nodes join the cluster.
✔️ Uses hostPath to access node logs.

7. Managing a DaemonSet

Check all running DaemonSet pods:

kubectl get daemonsets
kubectl get pods -o wide

Each node should have one Fluentd pod.

Manually adding a new node:

kubectl label node new-node app=fluentd

Now Fluentd will be scheduled on new-node.

8. Key Differences: StatefulSets vs. DaemonSets

Feature	StatefulSet	DaemonSet
Purpose	Manages stateful applications (e.g., databases).	Runs node-specific workloads (e.g., logging, monitoring).
Pod Identity	Each pod has a stable identity (pod-0, pod-1).	Each pod runs once per node.
Scaling	Adds/removes stateful replicas in order.	Automatically runs on all nodes.
Storage	Uses Persistent Volume Claims (PVCs).	Uses node storage (hostPath).
Common Use Cases	Databases, Kafka, Elasticsearch.	Fluentd, Prometheus, CNI plugins.



Kubernetes StatefulSets ensure stable pod identities and persistent storage, making them ideal for databases. DaemonSets, on the other hand, guarantee that a specific pod runs on every node, which is crucial for logging, monitoring, and networking services.

Understanding when to use StatefulSets vs. DaemonSets is critical for deploying complex, production-ready applications in Kubernetes.
