# Bootcamp-Final-Exam
#AWS token and secret have been assigned as GitHub secret variables.

# AWS EKS Cluster Deployment with Terraform and Helm

This project sets up an AWS EKS cluster using Terraform, with integration of Kubernetes and Helm providers. It also provisions IAM users and policies for administrative access.

## Prerequisites

- **AWS Account** with IAM permissions to create resources
- **Terraform** installed (>= 1.0.0)
- **AWS CLI** configured with credentials
- **kubectl** installed (for Kubernetes interaction)
- **Helm** installed (for Helm package management)

## Infrastructure Overview

### Terraform Providers
- **AWS Provider**: Manages AWS resources.
- **Kubernetes Provider**: Manages Kubernetes resources within the EKS cluster.
- **Helm Provider**: Deploys Helm charts within the Kubernetes cluster.

### Resources Deployed
1. **VPC Module**
   - Creates a Virtual Private Cloud (VPC) with public and private subnets.
   - Enables NAT gateway for private subnets.
   - Tags public and private subnets for Kubernetes load balancing.

2. **EKS Cluster Module**
   - Deploys an EKS cluster with version `1.28`.
   - Enables logging for API, audit, authenticator, controller manager, and scheduler.
   - Creates an EKS-managed node group with `t3.micro` instances.

3. **IAM User and Policies**
   - Creates an IAM user (`MurathanErgin`) with the following attached policies:
     - `AdministratorAccess`
     - `AmazonEKSClusterPolicy`
     - `AmazonEKSWorkerNodePolicy`

4. **Outputs**
   - IAM user ARN
   - EKS cluster name
   - EKS cluster endpoint

## Deployment Steps
```sh
git clone (https://github.com/merginn/Bootcamp-Final-Exam/)

### Step 2: Initialize Terraform
```sh
terraform init

### Step 3: Plan the Deployment
```sh
terraform plan

### Step 4: Apply the Deployment
```sh
terraform apply

### Step 5: Configure `kubectl` to Access the Cluster
```sh
aws eks update-kubeconfig --region eu-north-1 --name BootCamp

### Step 6: Verify Cluster Connection
```sh
kubectl get nodes

### Step 7: Deploy Helm Charts
```sh
helm repo add stable https://charts.helm.sh/stable
helm repo update

### Step 8: Verify Helm Deployment
```sh
helm list

### Step 9: Configure IAM User Access to EKS
Attach the necessary policies to allow user authentication:
```sh
aws iam attach-user-policy --user-name MurathanErgin --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

### Step 10: Grant Kubernetes Access
Modify the `aws-auth` ConfigMap to allow IAM user access:
```sh
kubectl edit configmap aws-auth -n kube-system

Add the following entry under `mapUsers`:
```sh
mapUsers: |
  - userarn: arn:aws:iam::ACCOUNT_ID:user/MurathanErgin
    username: MurathanErgin
    groups:
      - system:masters
### Step 11: Validate IAM User Access
Run the following command to ensure the IAM user has access:
```sh
kubectl get nodes --as MurathanErgin





