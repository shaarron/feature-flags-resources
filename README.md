# Feature Flags Resources

This repository contains Kubernetes manifests for deploying the feature flags service on EKS

#### Main components
- **Feature-flags-api** 
- **MongoDB** as the database
- **NGINX ingress controller** as the reverese proxy.
  
#### Observability 
- **Kube prometheus stack**
  - includes **prometheus**, **alertmanager**, **grafana**...   
- **EFK** 
  - **Elastic Cloud Kuberenets** for elasticsearch & kibana 
  - **Fluent-bit** for scraping 



## Feature Flags Stack - Umbrella Chart

The Feature Flags Stack is packaged as an umbrella Helm chart that orchestrates the deployment of multiple core components required for the **feature flags service** on EKS. 

This umbrella chart simplifies installation by combining several subcharts—such as **ingress-nginx**, **MongoDB**, **Feature Flags API**, and observability stacks (**Prometheus/Grafana** and **EFK**)—into a unified, environment-aware Helm deployment.

## Prerequisits 

### Namespaces

```bash
kubectl create namespace kps
kubectl create namespace efk
kubectl create namespace mongodb
```

### CRDs  & Operators


#### 1. KPS

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

```bash
kubectl apply --server-side -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_alertmanagers.yaml
kubectl apply --server-side -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_podmonitors.yaml
kubectl apply --server-side -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_probes.yaml
kubectl apply --server-side -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_prometheuses.yaml
kubectl apply --server-side -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_servicemonitors.yaml
kubectl apply --server-side -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_thanosrulers.yaml
```

#### 2. ECK - elastic cloud kubernetes (part of EFK - fluent bit will be installed later trough helm)

```bash
kubectl apply -f https://download.elastic.co/downloads/eck/3.1.0/crds.yaml


kubectl apply -f https://download.elastic.co/downloads/eck/3.1.0/operator.yaml
```

#### 3. Mongodb community operator

```bash
helm upgrade --install community-operator mongodb/community-operator \
  --set operator.version=0.13.0 \
  --set agent.version=12.0.25.7724-1

```




### Secrets


#### 1. ecr credentials - ?if repo is private, maybe i dont needit?

```bash
kubectl create secret docker-registry ecr-creds \           
  --docker-server=869432184848.dkr.ecr.ap-south-1.amazonaws.com \
  --docker-username=AWS \
  --docker-password="$(aws ecr get-login-password --region ap-south-1)" \
  --namespace default
```


2. mongodb secrets for both defaults & mongodb namespaces

```bash
kubectl create secret generic mongo-secret \                                     
  --from-literal=MONGO_INITDB_ROOT_PASSWORD="$(openssl rand -base64 18 | base64 -w0)" \
  --from-literal=APP_USER_PASSWORD="$(openssl rand -base64 18 | base64 -w0)"
```

 to get the passwords
```bash
 kubectl get secret mongo-secret -o jsonpath='{.data}' | jq 'map_values(@base64d)'
```

## Helm deploy 

#### commands

```bash
helm dependency update
```

```bash
helm upgrade --install feature-glags-stack .\helm\feature-flags-stack
```
