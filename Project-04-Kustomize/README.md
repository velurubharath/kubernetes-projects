Project 04 – Kubernetes with Kustomize (Base + Overlays)

This project demonstrates how to use Kustomize to manage Kubernetes manifests across multiple environments (Dev, Staging, Prod). Instead of duplicating YAML files, Kustomize lets us define a base and then apply environment-specific overlays.

📂 Project Structure
project-04-kustomize/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
├── overlays/
│   ├── dev/
│   │   ├── kustomization.yaml
│   │   └── replica-patch.yaml
│   └── prod/
│       ├── kustomization.yaml
│       └── resource-limits-patch.yaml

⚙️ Base Resources

Deployment.yaml – Defines the base application (e.g., Nginx or Flask app).

Service.yaml – Exposes the Deployment inside the cluster.

kustomization.yaml – Lists the base resources.

🔧 Overlays

Each environment modifies the base using patches:

Dev – Overrides replicas for lightweight deployments.


Prod – Adds resource limits and higher replicas.

🚀 How to Deploy
1. View the rendered manifests
kubectl kustomize overlays/dev
kubectl kustomize overlays/prod

2. Apply to cluster
kubectl apply -k overlays/dev
kubectl apply -k overlays/prod

✅ Example Commands

Check pods

kubectl get pods -n default


Check service

kubectl get svc


Check final YAML (Prod)

kubectl kustomize overlays/prod | less

🏆 Learning Outcomes

Understand Kustomize base + overlays.

Apply patches (replicas, images, resource limits).

Manage multi-environment deployments without duplicating YAML.

Build a real-world GitOps-style workflow.