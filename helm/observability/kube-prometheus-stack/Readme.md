# kube-prometheus-stack

This directory contains resources for deploying and managing the kube-prometheus-stack using Helm.

## Prerequisits

1. run
    ```bash
    helm dependency update ./helm/observability/kube-prometheus-stack 
    ```

    ```bash
    helm upgrade --install kps ./helm/observability/kube-prometheus-stack \
  -n kps --create-namespace
    ```

## Usage

1. Install the Helm chart:
    ```bash
    helm install [RELEASE_NAME] .
    ```

2. Customize values in `values.yaml` as needed.

## Resources

- [kube-prometheus-stack documentation](https://github.com/prometheus-operator/kube-prometheus)
