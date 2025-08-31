Project-07: Kubernetes Ingress 🚦

This project demonstrates how to use Kubernetes Ingress to expose multiple services (app1, app2) through a single Ingress controller.
Instead of accessing each service on random NodePorts, Ingress provides a unified entry point.

📂 Project Structure
project-07-ingress/
├── app1-deployment.yaml
├── app1-service.yaml
├── app2-deployment.yaml
├── app2-service.yaml
├── ingress.yaml
└── namespace.yaml

⚙️ Steps to Run
1. Create Namespace
kubectl apply -f namespace.yaml

2. Deploy Applications
kubectl apply -f app1-deployment.yaml
kubectl apply -f app1-service.yaml
kubectl apply -f app2-deployment.yaml
kubectl apply -f app2-service.yaml

3. Apply Ingress
kubectl apply -f ingress.yaml


This will configure routing rules:

http://<minikube-ip>:30080/app1 → forwards to app1-service

http://<minikube-ip>:30080/app2 → forwards to app2-service

🔧 Fixing Ingress Controller Service (NodePort)

By default, the ingress-nginx service may create random NodePorts.
Update it to fixed NodePorts:

kubectl edit svc ingress-nginx-controller -n ingress-nginx


Change the spec to:

spec:
  type: NodePort
  ports:
  - name: http
    port: 80
    targetPort: http
    protocol: TCP
    nodePort: 30080
  - name: https
    port: 443
    targetPort: https
    protocol: TCP
    nodePort: 30443


Verify:

kubectl get svc -n ingress-nginx


Expected:

ingress-nginx-controller   NodePort   10.96.210.156   <none>   80:30080/TCP,443:30443/TCP   10h

🌐 Accessing Apps

Get Minikube IP:

minikube ip


Example: 192.168.49.2

Access services:

curl http://192.168.49.2:30080/app1
curl http://192.168.49.2:30080/app2

📊 What You Learn

How Ingress works in Kubernetes.

Setting up multiple apps behind a single Ingress Controller.

Fixing random NodePorts by setting custom NodePort values.

Exposing apps via a single entry point for real-world scenarios.

✅ With this, you’ve implemented Ingress routing just like in production clusters where multiple apps are exposed behind a single domain.