# 📊 Project-05-Observability with Kubernetes

This project sets up an **observability stack** in Kubernetes using **Prometheus** and **Grafana** to monitor a sample application.  
The goal is to simulate **real-world monitoring practices** used in production clusters.

---

## 🚀 Project Overview
- Deploy a **sample application** exposing metrics in Prometheus format.
- Configure **Prometheus** to scrape metrics from the application.
- Deploy **Grafana** and import prebuilt dashboards for visualization.
- Learn how to troubleshoot missing metrics and validate monitoring setup.

---

## 🏗️ Architecture
┌──────────────┐
│ Demo App │ <-- Exposes metrics on port 9102 (/metrics)
└──────┬───────┘
│
▼
┌──────────────┐
│ Prometheus │ <-- Scrapes metrics from Demo App
└──────┬───────┘
│
▼
┌──────────────┐
│ Grafana │ <-- Dashboards and visualization
└──────────────┘


## ⚙️ Components

### 1️⃣ Demo Application
- Image: `prom/statsd-exporter:v0.26.1` (publicly available)
- Exposes metrics at: `http://<pod-ip>:9102/metrics`

### 2️⃣ Prometheus
- Configured via a **ConfigMap** (`prometheus-config`).
- Scrape job added for `demo-app`.

- job_name: 'demo-app'
  static_configs:
    - targets: ['demo-app:9102']
3️⃣ Grafana
Deployed as a StatefulSet/Deployment.

Exposes dashboards on http://<node-ip>:3000

Default credentials:

Username: admin

Password: admin

📦 Deployment Steps
Create Namespace

kubectl create ns observability
Deploy Prometheus

kubectl apply -f prometheus-config.yaml -n observability
kubectl apply -f prometheus-deployment.yaml -n observability
Deploy Demo App

kubectl apply -f demo-app-deployment.yaml -n observability
Deploy Grafana

kubectl apply -f grafana-deployment.yaml -n observability
Port Forward Grafana

kubectl port-forward svc/grafana 3000:3000 -n observability
Import Dashboards

Go to Grafana → Dashboards → Import.

Example IDs:

Prometheus 2.0 Stats → 3662

Kubernetes / Pods → 6417

✅ Validation
Check Prometheus UI:
kubectl port-forward svc/prometheus 9090:9090 -n observability
Visit → http://localhost:9090

Check Demo App metrics:

kubectl exec -it <demo-app-pod> -n observability -- curl http://localhost:9102/metrics
Grafana should show CPU, memory, requests, and custom metrics.