# Feature Flags App — GitOps Infrastructure

This repository contains the **infrastructure manifests** for deploying [**Feature Flags App**](https://github.com/shaarron/feature-flags-app). 


**Feature Flags App** running on **Amazon EKS**, managed with **Argo CD** and **Helm**, following modern GitOps principles - “App-of-Apps” pattern.

### componenets overview 
- **Core App**: Feature Flags API (with MongoDB & Nginx) 
- **Monitoring**: Kube-Prometheus-Stack (Prometheus, Grafana)  
- **Logging**: EFK Stack (Elasticsearch, Fluent Bit, Kibana)  


## Argocd Deployment Flow

```mermaid
graph TD
  A[**Root Application**] --> B[Bootstrap Layer]
  B --> B1[Namespaces]
  B --> B2[ECK Operator]
  B --> B3[MongoDB Operator]

  A --> C[Infrastructure Layer]
  C --> C1[Kube-Prometheus-Stack]
  C --> C2[EFK Stack]
  A --> D[Application Layer]
  D --> D1[Feature-Flags App + MongoDB]
```


## Stack Components order

| Component               | Namespace        | Sync Wave | Notes |
|------------------------|------------------|-----------|-------|
| Namespaces             | `argocd`         | `0`       | Ensures required namespaces exist |
| MongoDB Operator       | `mongodb`        | `0`       | Installs MongoDB Community Operator |
| ECK Operator           | `elastic-system` | `0`       | Deploys ECK operator for Elasticsearch & Kibana |
| Kube Prometheus Stack  | `kps`            | `2`       | Metrics stack (Prometheus, Grafana, etc.) |
| Feature Flags API      | `default`        | `3`       | Flask-based API for toggling flags |
| EFK Stack              | `efk`            | `3`       | Fluent Bit → Elasticsearch → Kibana |

---

## Argocd directory structure
 
1. **Bootstrap**
   - Create namespaces
   - Install ECK operator + crds
   - Install MongoDB operator + crds
 

2. **Infrastructure**
   - Deploy **kube-prometheus-stack** (**kps** namespace)
   - Deploy **EFK** (**efk** namespace)
     - [ECK](https://www.elastic.co/docs/deploy-manage/deploy/cloud-on-k8s) (**Elasic Cloud Kubernetes** - for Elasticsearch & kibana) 
     - Fluent Bit 

3. **Applications**
   - Deploy **Feature-Flags API** + MongoDB  
 


## Deploy Locally


```sh
# Install Argo CD (if not already installed)
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
t
# Apply the root app
kubectl apply -f argocd/root-app.yaml -n argocd

# Verify
kubectl get ns
kubectl get pods -A
kubectl get ingress -A

```

### Login to argocd CLI

```sh
# Get the password
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d; echo

# Login with the password from above
argocd login localhost:8080 --username admin --password <PASSWORD> --insecure
```
### Login trough UI 
```sh
# Port forward 
 kubectl port-forward svc/argocd-server -n argocd 8080:443 

# Get the password 
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d; echo

# Open argocd UI
# Login with admin user and the retrieved password
http://localhost:8080

```

