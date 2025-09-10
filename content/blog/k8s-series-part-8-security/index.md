+++
title = "Kubernetes RBAC (Role-Based Access Control) and Security"
date = "2025-03-14"
author = "Safique"
categories = ["Kubernetes"]
tags = ["Kubernetes", "RBAC", "Security", "K8s"]
draft = false
image = "images/blog/networkpolicy.png"
description = "Learn how Kubernetes RBAC manages access to resources, with examples of roles and permissions."
+++

In Kubernetes, securing your cluster and resources is paramount. **Role-Based Access Control (RBAC)** is a powerful tool for controlling access within Kubernetes. It allows administrators to define roles with specific permissions, ensuring that users or services can only access the resources they need.  

In this article, we'll dive into the key components of Kubernetes RBAC and explore how to implement security effectively.

---

#### Key RBAC Components

Kubernetes RBAC uses the following resources:  
1. **Roles** – Defines a set of permissions for resources within a namespace.  
2. **ClusterRoles** – Similar to roles but with cluster-wide scope.  
3. **RoleBindings** – Grants permissions defined in a role to a user, group, or service account.  
4. **ClusterRoleBindings** – Grants permissions to users, groups, or service accounts across the entire cluster.

---

#### Creating Roles and ClusterRoles

A **Role** is used to define access to resources within a specific namespace, while a **ClusterRole** is used to define cluster-wide access.

#### Example: Creating a Role to Read Pods
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list"]
```

This role grants read access to **pods** in the `default` namespace.

---

#### Example: Creating a ClusterRole to Administer All Resources
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin
rules:
  - apiGroups: [""]
    resources: ["*"]
    verbs: ["*"]
```

This **ClusterRole** grants full permissions on all resources cluster-wide.

---

#### Binding Roles to Users or Service Accounts

Once roles are created, they need to be assigned to users or service accounts. This is done via **RoleBindings** or **ClusterRoleBindings**.

##### Example: Binding the `pod-reader` Role to a User
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
  - kind: User
    name: "john"
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

This binds the `pod-reader` role to the user `john`.

---

#### 4. Best Practices for Kubernetes Security

- **Principle of Least Privilege:** Always grant the minimal set of permissions needed for a user or service to operate.  
- **Use Service Accounts:** Instead of binding users directly, bind **service accounts** to roles for better control.  
- **Review and Audit Permissions Regularly:** Regularly review roles and bindings to ensure users and services have the right level of access.

---

RBAC is a powerful feature for securing your Kubernetes resources by limiting access based on roles. By understanding how to create roles and bindings, you can ensure that your Kubernetes environment is both secure and well-governed.
