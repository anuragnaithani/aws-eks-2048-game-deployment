# Architecture

## High-Level Architecture

```text
Internet User
    ↓
AWS Application Load Balancer
    ↓
Kubernetes Ingress
    ↓
Kubernetes Service
    ↓
2048 Application Pods
    ↓
Amazon EKS on AWS Fargate
```

## Components

### Amazon EKS
Amazon EKS provides a managed Kubernetes control plane. AWS manages the Kubernetes master components such as API server and etcd.

### AWS Fargate
AWS Fargate runs Kubernetes pods without requiring EC2 worker node management.

### Kubernetes Deployment
Deployment manages application replicas and ensures the desired number of pods are running.

### Kubernetes Service
Service provides stable networking and forwards traffic to matching pods.

### Kubernetes Ingress
Ingress defines HTTP routing rules for external traffic.

### AWS Load Balancer Controller
The AWS Load Balancer Controller watches Ingress resources and creates AWS Application Load Balancers automatically.

### IAM OIDC Provider
IAM OIDC allows Kubernetes service accounts to assume IAM roles securely.
