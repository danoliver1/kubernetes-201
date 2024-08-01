

```bash
kubectl create namespace sticky-session-test
```{{exec}}

We want our pods to be "Istio enabled" so we must label the namespace. 

```bash
kubectl label namespace sticky-session-test istio-injection=enabled
```{{exec}}

This label will make sure our pods have an Istio sidecar.

Also we'll set the default namespace to be `sticky-session-test` so we don't have to add `-n sticky-session-test` to all of our commands

```bash
kubectl config set-context --current --namespace=sticky-session-test
```{{exec}}

```bash
helm install gateway istio/gateway -n istio-system
```{{exec}}