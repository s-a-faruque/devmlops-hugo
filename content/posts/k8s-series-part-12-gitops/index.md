+++
title = "CI/CD and GitOps in Kubernetes"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes", "DevOps"]
tags = ["Kubernetes", "CI/CD", "GitOps", "Helm", "Kustomize", "ArgoCD", "FluxCD", "Jenkins", "Tekton", "GitHub Actions"]
draft = true
description = "Learn how CI/CD and GitOps help automate Kubernetes deployments using Helm, Kustomize, ArgoCD, FluxCD, and CI/CD tools like Jenkins, Tekton, and GitHub Actions."
+++

Continuous Integration (CI) and Continuous Deployment (CD) are critical for modern DevOps workflows. Kubernetes (K8s) integrates well with CI/CD pipelines, allowing automated application deployments. **GitOps** takes this further by using **Git as the single source of truth** for Kubernetes infrastructure and application configurations.

This article covers:
- **Helm and Kustomize** for managing Kubernetes configurations.
- **ArgoCD and FluxCD** for GitOps-based deployments.
- **Jenkins, Tekton, and GitHub Actions** for automating CI/CD pipelines.

<!--more-->
---

### 1. Helm - The Kubernetes Package Manager

[Helm](https://helm.sh/) simplifies Kubernetes application deployment using **charts** (predefined YAML templates). It provides:
- Version control for Kubernetes applications.
- Reusable application configurations.
- Easy rollbacks.

### **Installing Helm**
```sh
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### **Example: Deploying an Nginx Chart**
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx
```
This command deploys an Nginx instance in Kubernetes.

### **Helm Values File (Override Defaults)**
Modify default configurations using a `values.yaml` file:
```yaml
replicaCount: 3
image:
  repository: nginx
  tag: latest
service:
  type: LoadBalancer
```
Then apply:
```sh
helm upgrade --install my-nginx bitnami/nginx -f values.yaml
```

---

## **2. Kustomize - Managing Kubernetes Configurations**  

[Kustomize](https://kustomize.io/) allows managing Kubernetes configurations without modifying the original YAML files. It enables:
- Overlays for different environments (dev, staging, prod).
- YAML modifications without duplication.
- Patch management.

### **Example: Kustomization for Different Environments**
```yaml
# kustomization.yaml
resources:
  - deployment.yaml
  - service.yaml

images:
  - name: my-app
    newTag: v2.0

patchesStrategicMerge:
  - deployment-patch.yaml
```
To apply:
```sh
kubectl apply -k .
```

---

## **3. GitOps - ArgoCD and FluxCD**  

GitOps enables **declarative infrastructure management** using Git repositories. **ArgoCD** and **FluxCD** are popular tools that sync Git changes to Kubernetes.

### **ArgoCD - Declarative Continuous Deployment**  
[ArgoCD](https://argo-cd.readthedocs.io/) continuously syncs Kubernetes clusters with Git repositories.

#### **Installing ArgoCD**
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

#### **Deploying an Application with ArgoCD**
```sh
argocd app create my-app \
  --repo https://github.com/user/repo.git \
  --path k8s \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
```
ArgoCD monitors the repository and applies changes automatically.

### **FluxCD - GitOps for Kubernetes**  
[FluxCD](https://fluxcd.io/) is another GitOps tool that automatically deploys Kubernetes manifests from Git.

#### **Installing FluxCD**
```sh
flux bootstrap github \
  --owner=my-user \
  --repository=my-repo \
  --branch=main \
  --path=clusters/my-cluster
```
FluxCD watches the repository and syncs updates to the cluster.

---

## **4. CI/CD Pipelines for Kubernetes**  

### **Jenkins for Kubernetes CI/CD**  
[Jenkins](https://www.jenkins.io/) is a popular CI/CD tool with Kubernetes support.

#### **Example: Jenkins Pipeline for Kubernetes Deployment**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t my-app:v1 .'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push my-registry/my-app:v1'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
```

### **Tekton - Kubernetes-Native CI/CD**  
[Tekton](https://tekton.dev/) is a cloud-native CI/CD system built for Kubernetes.

#### **Example: Tekton Pipeline**
```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: deploy-pipeline
spec:
  tasks:
    - name: build
      taskRef:
        name: build-task
    - name: deploy
      taskRef:
        name: deploy-task
      runAfter:
        - build
```
Tekton Pipelines run inside Kubernetes for better integration.

### **GitHub Actions for Kubernetes CI/CD**  
[GitHub Actions](https://github.com/features/actions) enables CI/CD workflows using YAML files.

#### **Example: Deploy to Kubernetes with GitHub Actions**
```yaml
name: Deploy to Kubernetes
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Set up kubectl
        uses: azure/setup-kubectl@v1
      - name: Apply Kubernetes manifests
        run: kubectl apply -f k8s/
```

---

## **5. Best Practices for Kubernetes CI/CD and GitOps**  

✅ **Use Helm or Kustomize** for managing configurations efficiently.  
✅ **Adopt GitOps** (ArgoCD or FluxCD) for declarative deployments.  
✅ **Automate CI/CD pipelines** with Jenkins, Tekton, or GitHub Actions.  
✅ **Ensure security** by restricting access to secrets and Kubernetes clusters.  
✅ **Monitor deployments** using Prometheus, Grafana, or Kubernetes Events.

---

CI/CD and GitOps streamline Kubernetes deployments by automating builds, testing, and releases.  
- **Use Helm or Kustomize** to manage configurations.  
- **Adopt GitOps** with **ArgoCD or FluxCD** for automated deployments.  
- **Set up CI/CD pipelines** with **Jenkins, Tekton, or GitHub Actions**.  

By following these best practices, teams can **deploy faster, improve reliability, and maintain Kubernetes clusters efficiently**.
