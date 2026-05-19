# Interview Explanation

## Short Explanation

I created an end-to-end Kubernetes deployment project on AWS EKS. In this project, I deployed a containerized 2048 game application using Kubernetes Deployment, Service, and Ingress.

I used Amazon EKS as the managed Kubernetes platform and AWS Fargate as the compute layer for running pods without managing EC2 worker nodes.

To expose the application publicly, I configured AWS Load Balancer Controller. The controller watched the Kubernetes Ingress resource and automatically created an AWS Application Load Balancer.

The traffic flow was:

```text
User → AWS ALB → Ingress → Kubernetes Service → Pods
```

I also configured IAM OIDC provider and IAM roles for service accounts so that the AWS Load Balancer Controller could securely communicate with AWS APIs.

This project helped me understand EKS, Fargate, Kubernetes networking, Ingress, AWS Load Balancer Controller, IAM OIDC integration, and production-style Kubernetes deployment.

---

## If Interviewer Asks: Why EKS?

EKS is a managed Kubernetes service. AWS manages the Kubernetes control plane, including API server availability, etcd, upgrades, and high availability. This reduces operational overhead and allows DevOps engineers to focus on application deployment and scaling.

---

## If Interviewer Asks: Why Ingress?

Ingress is used to route external HTTP traffic inside the Kubernetes cluster. Instead of creating a separate Load Balancer for every service, Ingress allows centralized routing using one Load Balancer Controller.

---

## If Interviewer Asks: What Was Your Role?

My role included:

- Creating the EKS cluster
- Configuring kubectl and eksctl
- Creating Fargate profiles
- Deploying the Kubernetes application
- Creating Service and Ingress resources
- Configuring IAM OIDC provider
- Installing AWS Load Balancer Controller
- Debugging controller issues
- Testing public access using ALB DNS
- Cleaning up resources to avoid AWS charges
