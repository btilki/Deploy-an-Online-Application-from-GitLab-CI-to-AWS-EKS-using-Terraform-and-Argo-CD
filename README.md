# Deploy an Online Application from GitLab CI to AWS EKS using Terraform and
  Argo CD

This repository provides a sample infrastructure-as-code workflow to deploy an
online application to an AWS EKS (Elastic Kubernetes Service) cluster using
GitLab CI/CD pipelines, Terraform for provisioning resources, and Argo CD for
GitOps-based application delivery.

## Overview

- **Infrastructure Provisioning:** Uses Terraform (written in HCL) to set up
    AWS resources, including the EKS cluster and supporting networking
    components.
- **CI/CD:** GitLab CI orchestrates automation pipelines: applying
    infrastructure code and triggering deployments.
- **Application Delivery:** Argo CD continuously synchronizes and manages the
    application’s Kubernetes manifests on the EKS cluster.

## Architecture

1. **Terraform** provisions:
   - AWS EKS cluster
   - VPC, subnets, IAM roles, security groups
   - Other required AWS infrastructure 2. **GitLab CI/CD pipeline**
     automates:
   - Terraform plan & apply
   - Docker image build and push (optional)
   - Triggering Argo CD app sync (via API or automated manifests update)
     3. **Argo CD** ensures:
   - GitOps workflow for application deployments
   - Continuous synchronization between Git repo and Kubernetes cluster

## Prerequisites

- AWS account & credentials
- GitLab project with CI enabled
- Terraform installed locally or as part of pipeline
- Access to EKS (kubectl, eksctl, AWS CLI)
- Argo CD instance deployed on EKS cluster

## Usage

1. **Clone this repo** ```bash git clone https://github.com/btilki/Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD.git cd
Deploy-an-Online-Application-from-GitLab-CI-to-AWS-EKS-using-Terraform-and-Argo-CD ```

2. **Configure Terraform variables**
   - Edit `terraform.tfvars` or relevant configuration files to match your AWS
     setup.

3. **Run Terraform (manually or automate via CI)** ```bash terraform init
terraform plan terraform apply ```

4. **Set up GitLab CI pipeline**
   - See `.gitlab-ci.yml` for pipeline configuration.
   - Configure CI/CD variables (AWS credentials, etc.) in GitLab.

5. **Deploy your application**
   - Push Kubernetes manifests or Helm charts to the Git repository monitored
     by Argo CD.
   - Argo CD will automatically sync and deploy to your EKS cluster.

## File Explanations

- **main-eks.tf:**  Defines the main AWS EKS (Elastic Kubernetes Service)
    cluster and its core resources. This includes the cluster itself, node
    groups, VPC and networking attachments, and relevant outputs such as
    endpoints or kubeconfigs.

- **.gitlab-ci.yml:**  Contains the GitLab CI/CD pipeline configuration.
     Defines the automated process for Terraform initialization, planning,
     and application of infrastructure changes, plus deployment stages for
     the application. Handles environment variables and secrets for AWS.

- **argocd.tf:**  Provisions Argo CD resources in the EKS cluster—typically
    using Terraform Helm or Kubernetes providers. Installs Argo CD, manages
    namespaces, and sets outputs(e.g., Argo CD server URL) for integration.

- **iam-role.tf:**  Declares AWS IAM roles and policies required for the EKS
    cluster and worker nodes. Ensures secure permissions for resources such
    as EC2, ECR, and cluster API, and manages the roles needed for worker
    nodes to operate properly.

- **kube-resources.tf:**  Manages Kubernetes resources needed by applications
    and Argo CD, such as namespaces, secrets, config maps, and possibly
    custom manifests. Uses the Terraform Kubernetes provider connected to the
    EKS cluster’s kubeconfig.

## References

- [Terraform AWS Provider]
  (https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS EKS Documentation]
  (https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)
- [Argo CD Docs](https://argo-cd.readthedocs.io/en/stable/)
- [GitLab CI/CD Pipeline](https://docs.gitlab.com/ee/ci/)

## License

MIT
