# MongoDB Helm Chart

This directory contains resources for deploying MongoDBCommunity using Helm.

## Prerequisits 
### 1. MongoDB operator installed 

 ```bash
helm repo add mongodb https://mongodb.github.io/helm-charts
```
```bash
helm repo update
```

```bash
helm upgrade --install community-operator mongodb/community-operator \
  --set operator.version=0.13.0 \
  --set agent.version=12.0.25.7724-1
```

Install the Helm chart:
```bash
helm install my-mongodb ./mongodb
```

