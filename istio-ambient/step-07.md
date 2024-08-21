

To utilize any [L7 features](https://istio.io/latest/docs/ambient/usage/l7-features/), a "waypoint proxy" must be deployed.

A waypoint serves as a "full-featured" Istio proxy, essentially the sidecar that was previously injected into pods.

A single waypoint is created for each namespace and this proxy will handle all Layer 7 traffic entering the namespace.

The waypoint is set up using the Gateway API.

```bash
kubectl apply -n webapp -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  labels:
    istio.io/waypoint-for: all
  name: webapp-waypoint
spec:
  gatewayClassName: istio-waypoint
  listeners:
  - name: mesh
    port: 15008
    protocol: HBONE
EOF
```{{exec}}

We must then label the namespace to instruct Istio to use the waypoint.

```bash
kubectl label namespace webapp istio.io/use-waypoint=webapp-waypoint
```{{exec}}

Please note: the label's value matches the gateway name.

After creating a waypoint we can then use L7 Authz Policies. For example, to allow only GET requests from the curl pod, or filtering on path, headers etc.

```bash
kubectl apply -n webapp -f - <<EOF
apiVersion: security.istio.io/v1
kind: AuthorizationPolicy
metadata:
  name: web-allow-get-from-curl
spec:
  targetRefs:
  - kind: Service
    group: ""
    name: web
  action: ALLOW
  rules:
  - from:
    - source:
        principals: ["cluster.local/ns/webapp/sa/curl"]
    to:
    - operation:
        methods: ["GET"]
EOF
```{{exec}}

After creating this authz policy with L7 feature (filtering on HTTP method), our request from the curl pod should still work

```bash
kubectl exec curl -n webapp -- curl web
```{{exec}}

However, a POST request will fail with "RBAC: access denied" because we only allowed GET requests

```bash
kubectl exec curl -n webapp -- curl web -X POST
```{{exec}}

Requests from any other pod will also fail, but instead of a connection reset, you'll now see "RBAC: access denied." This happens because the waypoint is functioning at the L7 layer.

```bash
kubectl run --rm -it --image curlimages/curl another-curl-pod -n webapp --restart=Never -- web
```{{exec}}