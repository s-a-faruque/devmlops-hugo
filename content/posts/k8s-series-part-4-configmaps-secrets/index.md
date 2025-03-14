+++
title = "Managing Kubernetes ConfigMaps and Secrets"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "K8s", "ConfigMaps", "Secrets", "Configuration Management"]
draft = false
description = "Learn how to manage configuration data securely in Kubernetes using ConfigMaps and Secrets."
+++

Introduction

Applications running in Kubernetes often require configuration settings such as database URLs, API keys, and environment variables. Kubernetes provides two key resources for managing this configuration data:
	1.	ConfigMaps – Store non-sensitive configuration data like environment variables and configuration files.
	2.	Secrets – Store sensitive information like passwords, tokens, and certificates securely.

1. What is a ConfigMap?

A ConfigMap allows you to store configuration data separately from your application code. This enables dynamic configuration updates without modifying container images.

Use Cases:
	•	Storing environment variables.
	•	Managing configuration files and command-line arguments.
	•	Decoupling application code from configuration.

Example: Creating a ConfigMap

Using YAML:

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  DATABASE_URL: "postgres://db-service:5432/mydb"

Apply the ConfigMap:

kubectl apply -f configmap.yaml

Using kubectl CLI:

kubectl create configmap app-config --from-literal=APP_ENV=production --from-literal=DATABASE_URL=postgres://db-service:5432/mydb

Using ConfigMap in a Pod

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      envFrom:
        - configMapRef:
            name: app-config

Verify the ConfigMap:

kubectl get configmap app-config -o yaml

2. What is a Secret?

A Secret is similar to a ConfigMap but is used for storing sensitive data like passwords, API keys, and certificates.

Use Cases:
	•	Storing database passwords securely.
	•	Managing API keys and tokens.
	•	Securing TLS certificates for encrypted communication.

Example: Creating a Secret

Using YAML (Base64 Encoded Values):

apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_USER: cG9zdGdyZXM=  # Base64 encoded "postgres"
  DB_PASSWORD: cGFzc3dvcmQ=  # Base64 encoded "password"

Apply the Secret:

kubectl apply -f secret.yaml

Using kubectl CLI:

kubectl create secret generic db-secret --from-literal=DB_USER=postgres --from-literal=DB_PASSWORD=password

Using a Secret in a Pod

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      env:
        - name: DB_USER
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_USER
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_PASSWORD

Verify the Secret:

kubectl get secret db-secret -o yaml

3. ConfigMap vs. Secret: Key Differences

Feature	ConfigMap	Secret
Purpose	Store non-sensitive config data	Store sensitive data securely
Data Type	Plain text	Base64-encoded
Security	Stored as plain text in etcd	Stored as base64-encoded data in etcd
Access	Environment variables, mounted as files	Environment variables, mounted as files

4. Best Practices for Using ConfigMaps and Secrets

✅ Use ConfigMaps for non-sensitive data like application settings.
✅ Use Secrets for sensitive data like credentials and API keys.
✅ Restrict access to Secrets using Role-Based Access Control (RBAC).
✅ Avoid hardcoding credentials in deployment files; use Secrets instead.
✅ Use mounted volumes for secrets instead of environment variables when security is a concern.



Kubernetes ConfigMaps and Secrets provide a way to manage configuration and sensitive data securely and efficiently. Understanding their differences and best practices ensures better application management in Kubernetes.

