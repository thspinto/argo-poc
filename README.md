# Argo Workflows PoC

This is a PoC demonstrating possible integrations using Argo Workflows and Argo Events.

1. Static Analysis pipelines with github global hooks

1. Scheduled DAG triggers with timezone support

1. Build using Drone file

## Architecture

TODO

## Running the PoC

1. Spin up a kubernetes of your choice.

1. Install All: Argo Events, Argo Workflows, Fission

```
kubectl apply -f kube/
```


To install from the master branch use:
```bash
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-events/master/hack/k8s/manifests/installation.yaml
kubectl create namespace argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo/stable/manifests/install.yaml
kubectl -n fission apply -f \
    https://github.com/fission/fission/releases/download/1.8.0/fission-all-1.8.0-minikube.yaml
```

This installation only reads argo events resources form the argo-events namespace.

References:

[Installing Argo Events](https://argoproj.github.io/argo-events/quick_start/)
[Installing Argo Workflows](https://github.com/argoproj/argo/blob/master/docs/getting-started.md)
[Installing Fission](https://docs.fission.io/docs/installation/#without-helm)

1. Install the [Argo Workflows cli](https://github.com/argoproj/argo/blob/master/docs/getting-started.md#1-download-the-argo-cli)

1. Install the [Fission cli](https://docs.fission.io/docs/installation/#install-fission-cli)

1. Argo Workflow Artifact Support

```bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
helm install argo-artifacts stable/minio --set service.type=LoadBalancer --set fullnameOverride=argo-artifacts
```

References:

[Configure Artifact Repository](https://github.com/argoproj/argo/blob/master/docs/configure-artifact-repository.md)

## Tasks

1. Triggering events using argo events

  * Show integration with Workflows (Github webhook)
```bash
kubectl -n argo-events -f http-workflow-sensor.yaml
kubectl -n port-forward svc/webhook-gateway-svc 12000:12000
curl -d '{"message":"this is my first webhook"}' -H "Content-Type: application/json" -X POST http://localhost:12000/example
```
To check the workflow status and logs run:
```bash
argo list -n argo-events
argo -n argo-events logs <workflow-id>
```

  * Show integration with Fission (Kafka or SQS)


1. Global CI pipelines

  * Use Github webhooks to trigger a pipeline (would be nice to support Drone's YAML)
  * Scan for secrets on Github webhook

1. Scheduled jobs

  * Trigger a daemons job using argo events
