# 🚀 Project 2 – Kubernetes Scheduling

This project demonstrates how Kubernetes schedules Pods across Nodes and how we can influence scheduling behavior using advanced concepts like **NodeSelectors, Affinity, Taints & Tolerations, and Priority Classes**.

---

## 📌 Topics Covered
1. **Node Selector** – Schedule Pods on specific nodes.  
2. **Node Affinity** – Flexible scheduling rules with expressions.  
3. **Taints & Tolerations** – Control which Pods can be scheduled on tainted nodes.  
4. **Pod Affinity & Anti-Affinity** – Place Pods together or apart.  
5. **Resource-based Scheduling** – Schedule based on CPU/Memory requests.  
6. **Pod Priority & Preemption** – Decide which Pods run when resources are scarce.  

---

## 📂 File Structure
project-02-scheduling/
│── node-selector.yaml
│── node-affinity.yaml
│── taints-tolerations.yaml
│── pod-affinity.yaml
│── priority-preemption.yaml
│── README.md


---

## ⚡ Examples

### 1️⃣ Node Selector
```yaml
nodeSelector:
  disktype: ssd
👉 Pod runs only on nodes labeled disktype=ssd.
Label your node:

kubectl label nodes <node-name> disktype=ssd

🔍 Verify Scheduling

Check scheduling decisions:

kubectl describe pod <pod-name>


Check node labels:

kubectl get nodes --show-labels


Check taints:

kubectl describe node <node-name> | grep Taints