# EFK Stack Observability

This document provides an overview of the EFK (Elasticsearch, Fluentd, Kibana) stack and some example commands to help you manage and troubleshoot your deployment.

## Example Commands

### Prerequisits
1. install crds 
    ```bash
    kubectl apply -f https://download.elastic.co/downloads/eck/3.1.0/crds.yaml
    kubectl apply -f https://download.elastic.co/downloads/eck/3.1.0/operator.yaml
    ```
2. Run helm commands
    ```bash
    helm dependency build ./helm/observability/efk
    ```

    ```bash
     helm upgrade --install efk ./helm/observability/efk  -n efk --create-namespace
    ```k

### connect to kibana
user: elastic 
password:
kubectl get secret elasticsearch-es-elastic-user -n efk -o jsonpath="{.data.elastic}" | base64 --decode; echo 