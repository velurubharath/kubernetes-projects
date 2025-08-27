📦 Project-03-Helm – Multi-Environment Helm Deployment with GitHub Actions

This project demonstrates real-world Kubernetes deployments using Helm with environment-specific configurations (Dev / Prod) and CI/CD automation via GitHub Actions.

📂 Project Structure
project-03-helm/
  ├── charts/
  │    └── myapp/                # Custom Helm chart
  │         ├── templates/       # Deployment, Service, Ingress
  │         ├── values.yaml      # Default values
  │         ├── values-dev.yaml  # Dev environment overrides
  │         ├── values-prod.yaml # Prod environment overrides
  ├── .github/
  │    └── workflows/
  │         └── deploy.yaml      # GitHub Actions CI/CD pipeline
  ├── README.md

⚙️ Features

Helm chart for Kubernetes app deployment

Configurable values per environment (dev / prod)

Ingress support for routing traffic to services

GitHub Actions CI/CD:

    Push to dev → Deploys to Dev cluster

    Push to main → Deploys to Prod cluster

Best practices:

     linting & dry runs

    Namespace isolation (dev, prod)

    Easy rollback with helm rollback

🚀 Deployment Steps
1️⃣ Install Dependencies

Make sure you have the following installed:

kubectl version --client
helm version

2️⃣ Deploy with Helm Locally
# Dev environment
helm upgrade --install myapp ./charts/myapp -f charts/myapp/values-dev.yaml --namespace dev --create-namespace

# Prod environment
helm upgrade --install myapp ./charts/myapp -f charts/myapp/values-prod.yaml --namespace prod --create-namespace

3️⃣ CI/CD Pipeline (GitHub Actions)

Workflow: .github/workflows/deploy.yaml

On push to dev branch → Deploys to Dev cluster using values-dev.yaml

On push to main branch → Deploys to Prod cluster using values-prod.yaml

🔐 GitHub Secrets

Add these secrets in your repo:

KUBECONFIG_DEV → base64 encoded kubeconfig for Dev cluster

KUBECONFIG_PROD → base64 encoded kubeconfig for Prod cluster

Generate with:

cat ~/.kube/config | base64

📊 Monitoring & Rollbacks

Check release history:

helm history myapp -n dev


Rollback to previous version:

helm rollback myapp <REVISION> -n dev

✅ Expected Outcome

Dev cluster gets latest features from dev branch.

Prod cluster remains stable, only updated when main is merged.

CI/CD handles linting, dry-run, and deployment automatically.

Rollback can be triggered easily in case of failures.

🏢 Real-World Relevance

This setup mimics how top-tier companies (Amazon, EPAM, ThoughtWorks) handle:

Multi-env deployments with Helm

GitHub Actions CI/CD pipelines

Environment-specific overrides

Rollbacks & monitoring in production