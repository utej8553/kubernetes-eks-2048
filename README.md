# Deploying the 2048 Game on Amazon EKS (CLI Workflow)

## Prerequisites

* AWS CLI configured
* `kubectl` installed
* `eksctl` installed
* Docker installed (optional)
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

Contents:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-2048
spec:
  replicas: 1
  selector:
    matchLabels:
      app: game-2048
  template:
    metadata:
      labels:
        app: game-2048
    spec:
      containers:
      - name: game-2048
        image: public.ecr.aws/l6m2t8p7/docker-2048:latest
        ports:
        - containerPort: 80
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

Contents:

```
apiVersion: v1
kind: Service
metadata:
  name: mygame-svc
spec:
  selector:
    app: game-2048
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
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

Example:

```
http://ac14d12d01bbf488b92c605b9d86f3e7-1479432683.us-east-2.elb.amazonaws.com
```

---

# 12. Check All Cluster Resources

```
kubectl get all
```

---

# 13. Check Pods in All Namespaces

```
kubectl get pods -A
```

---

# 14. Check Pod Labels

```
kubectl get pods --show-labels
```

---

# 15. Describe Pod for Debugging

```
kubectl describe pod <pod-name>
```

---

# 16. View Pod Logs

```
kubectl logs <pod-name>
```

---

# 17. Check Pod Placement

```
kubectl get pods -o wide
```

---

# 18. Delete Deployment

```
kubectl delete deployment deployment-2048
```

---

# 19. Delete Service

```
kubectl delete svc mygame-svc
```

---

# 20. Delete EKS Cluster

```
eksctl delete cluster --name demo-cluster --region us-east-2
```
