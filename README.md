# EKS Cluster with NGINX Ingress via Terraform and Helm

This project provisions an EKS cluster using Terraform and deploys an NGINX ingress controller via the Helm provider. The setup is split into two folders for clarity and separation of concerns. Everything is based on official Terraform modules and Helm charts.

---

## 📁 Folder Structure

- `learn-terraform-provision-eks-cluster/` – Provisions the EKS cluster and supporting infrastructure  
- `learn-terraform-helm/` – Deploys the NGINX ingress controller using the Helm provider

---

## ✅ Prerequisites

Ensure the following tools are installed and configured:

- [Terraform CLI](https://developer.hashicorp.com/terraform/downloads) (>= 1.0.0)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) (with credentials configured)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Helm v3](https://helm.sh/docs/intro/install/) (**required** – see note below)

---

## 🚀 Deployment Steps

### 1. Provision the EKS Cluster

```bash
cd learn-terraform-provision-eks-cluster
terraform init
terraform apply
```

Terraform will:
- Create a new VPC and subnets
- Set up IAM roles
- Provision the EKS cluster with node groups

---

### 2. Update kubeconfig to Access the Cluster

After the EKS cluster is created, update your local kubeconfig so that `kubectl` can talk to the cluster:

```bash
aws eks update-kubeconfig --region <your-region> --name <eks-cluster-name>
```

Replace `<your-region>` and `<eks-cluster-name>` with values shown in your Terraform output.

---

### 3. Deploy NGINX via Helm

```bash
cd ../learn-terraform-helm
terraform init
terraform apply
```

This will deploy the NGINX web server into the EKS cluster using the bitnami Helm chart.

---

### 4. Verify Deployment

Check that everything is running as expected:

```bash
kubectl get all -n <nginx-namespace>
```

Replace `<nginx-namespace>` with the namespace used in the Helm release (default is usually `ingress-nginx`).

---

## 💡 Design Notes

This setup intentionally reuses official Terraform modules and Helm charts to avoid unnecessary complexity.

> **Why reinvent the wheel?**  
> The goal was to demonstrate infrastructure-as-code practices, not to rebuild proven tools from scratch. Using official modules ensures better reliability, compatibility, and maintainability.

### ⚠️ Helm v3 Required

Terraform’s Helm provider failed during deployment when an older version of Helm (v2) was installed. Switching to **Helm v3** resolved the issue. Make sure you're using Helm v3, which is the current standard and supported by Terraform.

---

## 📬 Questions?

Feel free to reach out or raise an issue if anything is unclear or if you want help modifying this setup.
