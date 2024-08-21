

With Istio Ambient the [Gateway API](https://gateway-api.sigs.k8s.io/) is used to create waypoints (explanation and example later) so we must install the Gateway API CRDs

```
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.1.0/standard-install.yaml
```{{exec}}