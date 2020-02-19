# Argo Workflows PoC

This is a PoC demonstrating possible integrations using Argo Workflows and Argo Events.

1. Static Analysis pipelines with github global hooks

1. Scheduled DAG triggers with timezone support

1. Build using Drone file

## Architecture


## Running the PoC

1. Spin up a kubernetes of your choice.

1. Install Argo Events And Workflows

```
kubectl apply -f kube/
```

To install from the master branch use:
```bash
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-events/master/hack/k8s/manifests/installation.yaml
kubectl create namespace argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo/stable/manifests/install.yaml
```

This installation only reads argo events resources form the argo-events namespace.

References:

[Installing Argo Events](https://argoproj.github.io/argo-events/quick_start/)
[Intalling Argo Workflows](https://github.com/argoproj/argo/blob/master/docs/getting-started.md)


1. Install the [Argo Workflows cli](https://github.com/argoproj/argo/blob/master/docs/getting-started.md#1-download-the-argo-cli)

1. Argo Workflow Artifact Support

```bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
helm install argo-artifacts stable/minio --set service.type=LoadBalancer --set fullnameOverride=argo-artifacts
```

References:

[Configure Artifact Repository](https://github.com/argoproj/argo/blob/master/docs/configure-artifact-repository.md)
