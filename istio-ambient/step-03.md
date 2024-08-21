

```bash
kubectl create namespace webapp
```{{exec}}

To enable Istio in our namespace we need to add a label

```bash
kubectl label namespace webapp istio.io/dataplane-mode=ambient
```{{exec}}

We'll also force mTLS so we can be sure that requests are encrypted in transit

```bash
kubectl apply -n webapp -f - <<EOF
apiVersion: security.istio.io/v1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF
```{{exec}}

