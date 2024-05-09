
Lets create a namespace to put our example app in

```bash
kubectl create ns purple-team
```{{exec}}

We want our pods to be "Istio enabled" so we must label the namespace. 

```bash
kubectl label namespace purple-team istio-injection=enabled
```{{exec}}

This label will make sure our pods have an Istio sidecar.
