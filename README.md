# AWS EKS End-to-End Kubernetes Project

This project demonstrates an end-to-end Kubernetes application deployment on **Amazon EKS** using **AWS Fargate**, **Kubernetes Ingress**, **AWS Load Balancer Controller**, and **Application Load Balancer (ALB)**.

The deployed application is the 2048 game, exposed publicly through an AWS Application Load Balancer.

---

## Project Architecture

```text
User / Browser
      ↓
AWS Application Load Balancer (ALB)
      ↓
Kubernetes Ingress
      ↓
Kubernetes Service
      ↓
Pods running 2048 Game Application
      ↓
Amazon EKS Cluster on AWS Fargate
```

---

## Technologies Used

- Amazon EKS
- Kubernetes
- AWS Fargate
- AWS Load Balancer Controller
- AWS Application Load Balancer
- IAM OIDC Provider
- IAM Roles for Service Accounts
- kubectl
- eksctl
- Helm
- AWS CLI

---

## Project Workflow

1. Installed required tools: AWS CLI, kubectl, eksctl, and Helm.
2. Created an Amazon EKS cluster using eksctl with Fargate.
3. Configured kubectl to connect with the EKS cluster.
4. Created a Fargate profile for the application namespace.
5. Deployed the 2048 game using Kubernetes Deployment, Service, and Ingress.
6. Configured IAM OIDC provider for the EKS cluster.
7. Created IAM policy and IAM service account for AWS Load Balancer Controller.
8. Installed AWS Load Balancer Controller using Helm.
9. Exposed the application publicly using AWS ALB.
10. Verified the application using the ALB DNS URL.
11. Cleaned up AWS resources after completing the demo.

---

## Commands Used

All commands are available in:

```text
commands/setup-commands.txt
```

Cleanup commands are available in:

```text
commands/cleanup-commands.txt
```

---

## Kubernetes Manifest

The Kubernetes application manifest is available in:

```text
yaml/2048_full.yaml
```

---

## Screenshots

Add your screenshots inside the `screenshots/` folder.

Recommended screenshots:

| Screenshot | Description |
|---|---|
| `01-eks-cluster.png` | EKS cluster overview |
| `02-fargate-profiles.png` | Fargate profiles |
| `03-pods-running.png` | Kubernetes pods running |
| `04-alb.png` | AWS Application Load Balancer |
| `05-target-group.png` | Healthy target group |
| `06-ingress.png` | Kubernetes Ingress output |
| `07-application-running.png` | 2048 game running in browser |

---

## Important Commands

### Create EKS Cluster

```bash
eksctl create cluster --name demo-cluster --region us-east-1 --fargate
```

### Configure kubectl

```bash
aws eks update-kubeconfig --name demo-cluster --region us-east-1
```

### Create Fargate Profile

```bash
eksctl create fargateprofile \
  --cluster demo-cluster \
  --region us-east-1 \
  --name alb-sample-app \
  --namespace game-2048
```

### Deploy 2048 Application

```bash
kubectl apply -f yaml/2048_full.yaml
```

### Check Pods

```bash
kubectl get pods -n game-2048
```

### Check Ingress

```bash
kubectl get ingress -n game-2048
```

---

## Troubleshooting Faced

During the project, the AWS Load Balancer Controller initially went into `CrashLoopBackOff` because it could not fetch the VPC ID from EC2 metadata while running on Fargate.

The issue was fixed by explicitly adding the VPC ID argument to the controller deployment:

```bash
kubectl patch deployment aws-load-balancer-controller \
  -n kube-system \
  --type='json' \
  -p='[
    {
      "op":"add",
      "path":"/spec/template/spec/containers/0/args/-",
      "value":"--aws-vpc-id=<your-vpc-id>"
    }
  ]'
```

---

## Cleanup

To avoid AWS charges, delete the cluster after the demo:

```bash
eksctl delete cluster --name demo-cluster --region us-east-1
```

Verify cleanup:

```bash
aws eks list-clusters --region us-east-1
aws elbv2 describe-load-balancers --region us-east-1
```

Expected output:

```text
clusters: []
LoadBalancers: []
```

---

## Learning Outcomes

- Understood Amazon EKS architecture.
- Learned Kubernetes Deployment, Service, and Ingress.
- Configured AWS Load Balancer Controller.
- Integrated IAM OIDC with Kubernetes service accounts.
- Exposed a Kubernetes application publicly using ALB.
- Learned how to debug real-world EKS and Fargate issues.

---

## Resume Line

Deployed a containerized application on Amazon EKS using Kubernetes, AWS Fargate, Ingress, IAM OIDC integration, and AWS Load Balancer Controller.
