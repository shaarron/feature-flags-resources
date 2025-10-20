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
  -n mongodb --create-namespace \
  --set operator.version=0.13.0 \
  --set agent.version=12.0.25.7724-1
```
### 2. Create secret
create a secret with base64 encoded values 

```bash
kubectl create secret generic mongo-secret -n mongodb \
  --from-literal=MONGO_INITDB_ROOT_PASSWORD='<root_user__password>' \
  --from-literal=APP_USER_PASSWORD='app_user_password'
```

1. Install the Helm chart:
    ```bash
    helm install my-mongodb ./mongodb
    ```

