# Feature Flags Resources

This repository contains Kubernetes manifests for deploying a feature flags service, including MongoDB as the database and an NGINX ingress controller as the reverese proxy.

## Project Structure

- **`feature-flags-api/`**: Contains manifests for deploying the feature flags API service.
- **`mongo/`**: Contains manifests for deploying MongoDB, including ConfigMap, Secrets, StatefulSet, PVC, and Service.
- **`ingress-nginx/`**: Contains manifests for configuring the NGINX ingress controller and deploying an ingress resource.

## Prerequisites


- A Kubernetes cluster
- `kubectl` CLI configured to interact with your cluster

### Secrets 
- Base64-encoded values for MongoDB credentials (update `mongo/secrets.yaml` accordingly)
- ECR credentials - ensure ecr-creds secret exists:
```sh
kubectl create secret docker-registry ecr-creds \
  --docker-server=869432184848.dkr.ecr.ap-south-1.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password --region ap-south-1) \
  --docker-email=your-email@example.com
```
## Applying Resources

Apply the resources in the following order to ensure proper setup:

### 1. MongoDB Resources
 **! Prerequisite** - Update `mongo/secrets.yaml` with your base64-encoded MongoDB credentials before applying.

Apply the resources in the `mongo/` directory in the following order:

1. **MongoDB resources**: Defines MongoDB configuration.

   ```sh
   cd mongo/
   kubectl apply -f .mongo/
   ```

### 2. Feature Flags API
Apply the resources in the `feature-flags-api/` directory to deploy the API service:

1. **Deployment**: Deploys the API service.

   ```sh
   cd feature-flags-api/
   kubectl apply -f .
   ```

### 3. Ingress Controller
Configure the NGINX ingress controller with helm and custom values.yaml
   ```sh
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   helm update 
   helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx -f ingress-nginx/values.yaml
   ```

Apply the ingress resource
   ```sh
   kubectl apply -f ingress-nginx/ingress.yaml
   ```
### 4. Access API 
API should be available:
http://localhost/flags



## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.