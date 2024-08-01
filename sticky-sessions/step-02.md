

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

Finally we'll install an Istio Gateway and configure it

```bash
helm install sticky-session-test-gateway istio/gateway --set service.type=ClusterIP
```{{exec}}

```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1beta1
kind: Gateway
metadata:
  name: sticky-session-test-gateway
spec:
  selector:
    istio: sticky-session-test-gateway
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"
EOF
```{{exec}}