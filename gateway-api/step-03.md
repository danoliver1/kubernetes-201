
With our **Cluster Operator** hat still on, the Gateway API CRDs need to be installed so **Platform Engineers** and **Development Teams** can create `Gateway` and `HttpRoute` resources.

To install the Gateway API CRDs run
```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.0.0/standard-install.yaml
```{{exec}}
