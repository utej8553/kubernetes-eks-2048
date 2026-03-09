# 2048 on EKS

* AWS CLI 
* `kubectl` 
* `eksctl` 
* AWS IAM user with sufficient permissions

---

# 1. Create an EKS Cluster

```
eksctl create cluster --name demo-cluster --region us-east-2 --nodes 1 --node-type t3.micro
```

---

# 2. Configure kubectl for the Cluster

```
aws eks update-kubeconfig --region us-east-2 --name demo-cluster
```

---

# 3. Verify Cluster Nodes

```
kubectl get nodes
```

---

# 4. Create Deployment YAML

Create a file:

```
2048-deployment.yaml
```
---

# 5. Deploy Application

```
kubectl apply -f 2048-deployment.yaml
```

---

# 6. Verify Pod Status

```
kubectl get pods
```

---

# 7. Create Service YAML

Create file:

```
mygame-svc.yaml
```
---

# 8. Create LoadBalancer Service

```
kubectl apply -f mygame-svc.yaml
```

---

# 9. Check Service Status

```
kubectl get svc
```

---

# 10. Verify Service Endpoints

```
kubectl get endpoints
```

---

# 11. Access Application

```
http://<EXTERNAL-LOADBALANCER-DNS>
```

