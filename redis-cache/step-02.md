

```bash
kubectl create namespace redis-test
```{{exec}}

We want our pods to be "Istio enabled" so we must label the namespace. 

```bash
kubectl label namespace redis-test istio-injection=enabled
```{{exec}}

This label will make sure our pods have an Istio sidecar.

Also we'll set the default namespace to be `redis-test` so we don't have to add `-n redis-test` to all of our commands

```bash
kubectl config set-context --current --namespace=redis-test
```{{exec}}

Finally we will enable mTLS strict mode so Istio enabled pods will only allow secure communication using mTLS

```bash
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF
```{{exec}}