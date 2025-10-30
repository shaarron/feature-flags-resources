

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
    -n mongodb --create-namespace \
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
  kubectl create secret generic mongo-secret -n mongodb \
    --from-literal=MONGO_INITDB_ROOT_PASSWORD='<root_user__password>' \
    --from-literal=APP_USER_PASSWORD='<app_user_password>'
```


```bash
  kubectl create secret generic mongo-secret -n default \
    --from-literal=MONGO_INITDB_ROOT_PASSWORD='<root_user__password>' \
    --from-literal=APP_USER_PASSWORD='<app_user_password>'
```


## Helm deploy 

#### commands

```bash
  helm dependency update
```

```bash
helm upgrade --install feature-glags-stack .\helm\feature-flags-stack
```
