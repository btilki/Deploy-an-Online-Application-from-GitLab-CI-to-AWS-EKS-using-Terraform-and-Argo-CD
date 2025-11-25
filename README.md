# Deploy an Online Application from GitLab CI to AWS EKS using Terraform and
  Argo CD

This repository contains all the necessary infrastructure-as-code(IaC) scripts
and configuration for deploying an online application to AWS EKS
(Elastic Kubernetes Service) using Terraform and Argo CD, with integration
through GitLab CI.

## Overview

- **Terraform** provisions and configures AWS resources including VPC, EKS
    Cluster, IAM roles, and supporting infrastructure.
- **Argo CD** enables GitOps continuous delivery, managing application
    deployment to the EKS cluster.
- **GitLab CI** handles the CI pipeline, automating and orchestrating
    deployments and infrastructure changes.

## Repository Structure

All files in this repository are written in [HCL (HashiCorp Configuration
Language)](https://github.com/hashicorp/hcl). A typical structure includes:

## Key Terraform Modules and Resources

This repository is structured with modular, purpose-focused Terraform files:

### [`main-eks.tf`]
    (https://github.com/btilki/Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD/blob/main-eks.tf)
    Defines the core AWS infrastructure:
- AWS provider configuration.
- VPC creation with public/private subnets and tagging for Kubernetes.
- EKS Cluster setup: Node group management, cluster version, public endpoint,
  and base add-ons (kube-proxy, vpc-cni, coredns).
- Integrates EKS with cluster blueprints and advanced add-ons like the AWS
  Load Balancer Controller, Metrics Server, and Cluster Autoscaler.

### [`kube-resources.tf`]
    (https://github.com/btilki/Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD/blob/main/kube-resources.tf)
    Manages Kubernetes resources using Terraform:
- Creates application-specific namespaces (e.g., `online-boutique`).
- Sets up cluster roles and namespace viewer roles for RBAC.
- Manages bindings between roles, namespaces, and users for secure access.

### [`iam-roles.tf`]
    (https://github.com/btilki/Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD/blob/main/iam-roles.tf)
    Defines IAM roles and policies for AWS and Kubernetes access management:
- Configures administrator/developer roles with role assumption policies.
- Maps AWS IAM roles to Kubernetes service accounts and users
  (RBAC integration).
- Utilizes finely tuned resource policies for secure and least-privilege
  access.

### [`argocd.tf`]
    (https://github.com/btilki/Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD/blob/main/argocd.tf)
    Installs and configures Argo CD for GitOps-driven deployments:
- Provisions the Argo CD Helm release into EKS.
- Sets up GitOps repository secrets for secure access.
- Manages the Argo CD application for automatic sync/deployment from GitLab to
  EKS.

## Prerequisites

- AWS account credentials
- Terraform v1.0+ installed
- kubectl installed
- Access to a GitLab instance with CI/CD enabled
- Optional: Helm for Argo CD installation

## Getting Started

1. **Clone the Repository:** ```bash git clone
https://github.com/btilki/Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD.git
cd
Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD
```

2. **Configure Variables:**
   - Edit `variables.tf` with your AWS settings and parameters.

3. **Initialize Terraform:** ```bash terraform init ```

4. **Plan & Apply Infrastructure:** ```bash terraform plan terraform apply ```

5. **Configure Argo CD:**
   - Deploy Argo CD to EKS using Helm or manifests.
   - Connect Argo CD to your application's Git repository.

6. **Set Up GitLab CI:**
   - Add a `.gitlab-ci.yml` to trigger both Terraform and Argo CD workflows.

## Usage

- Push updates to your infrastructure or Kubernetes manifests to trigger
  CI/CD.
- Monitor deployments in Argo CD and GitLab CI.

## Resources & References

- [Terraform Documentation](https://www.terraform.io/docs)
- [AWS EKS Documentation](https://docs.aws.amazon.com/eks/)
- [Argo CD Documentation](https://argo-cd.readthedocs.io/)
- [GitLab CI/CD Docs](https://docs.gitlab.com/ee/ci/)

## License

MIT License.


