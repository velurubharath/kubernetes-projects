📘 Project 06 – Kubernetes Logging with EFK Stack

This project sets up centralized logging in Kubernetes using the EFK stack:

Elasticsearch → stores logs

Fluentd → log collector and forwarder

Kibana → visualization and search

📂 Project Structure
project-06-logging/
│── elasticsearch-deployment.yaml
│── fluentd-daemonset.yaml
│── kibana-deployment.yaml
│── namespace.yaml
│── README.md

🚀 Steps to Deploy
1️⃣ Create a Namespace
kubectl create ns logging

2️⃣ Deploy Elasticsearch
kubectl apply -f elasticsearch-deployment.yaml -n logging


⚠️ Note: Elasticsearch is memory-hungry. Set ES_JAVA_OPTS and resource limits:

env:
- name: ES_JAVA_OPTS
  value: "-Xms512m -Xmx512m"
resources:
  requests:
    memory: "1Gi"
    cpu: "500m"
  limits:
    memory: "1Gi"
    cpu: "1"

3️⃣ Deploy Fluentd (DaemonSet)
kubectl apply -f fluentd-daemonset.yaml -n logging


Fluentd will run on each node and forward logs to Elasticsearch.

4️⃣ Deploy Kibana
kubectl apply -f kibana-deployment.yaml -n logging

5️⃣ Verify Pods
kubectl get pods -n logging

6️⃣ Access Kibana

Forward Kibana service to localhost:

kubectl port-forward svc/kibana 5601:5601 -n logging


Open → http://localhost:5601

🔎 Expected Output

kubectl get pods -n logging should show Running pods for elasticsearch, fluentd, and kibana.

In Kibana, you can configure an index pattern logstash-* to start viewing logs from Kubernetes pods.

🛠 Troubleshooting

If Elasticsearch pod is OOMKilled → increase memory limits or reduce JVM heap (ES_JAVA_OPTS).

If Kibana shows no data → ensure Fluentd DaemonSet is running and pushing logs.

Check logs:

kubectl logs <fluentd-pod> -n logging
kubectl logs <elasticsearch-pod> -n logging

🎯 Learning Outcomes

✔ Understand how logs flow in Kubernetes from pods → Fluentd → Elasticsearch → Kibana
✔ Learn how to configure resource limits to avoid OOMKilled issues
✔ Gain hands-on with one of the most widely used logging stacks in production