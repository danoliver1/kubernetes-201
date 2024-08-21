

Currently our web server is available to any Istio enabled pods in the cluster. 

We can confirm this by creating a temporary curl pod in the same namespace.

```bash
kubectl run --rm -it --image curlimages/curl another-curl-pod -n webapp --restart=Never -- web
```{{exec}}

This should succeed. We haven't created an authz policy yet so all requests are allowed from any Istio enabled pod.

Let's create an authz policy attached to our web server to only allow requests from our `curl` pod.

```bash
kubectl apply -n webapp -f - <<EOF
apiVersion: security.istio.io/v1
kind: AuthorizationPolicy
metadata:
  name: web-allow-from-curl
spec:
  selector:
    matchLabels:
      app: web
  action: ALLOW
  rules:
  - from:
    - source:
        principals: ["cluster.local/ns/webapp/sa/curl"]
EOF
```{{exec}}

If we retry the command from above, it will now fail with "Connection reset by peer"

```bash
kubectl run --rm -it --image curlimages/curl another-curl-pod -n webapp --restart=Never -- web
```{{exec}}

However, our `curl` pod should still work as we have allowed requests from `curl`.

```bash
kubectl exec curl -n webapp -- curl web
```{{exec}}

To summarise, we created an authorization policy to only allow requests from our `curl` pod to our `web` pod.

Let's clean up:

```bash
kubectl delete ap -n webapp web-allow-from-curl
```{{exec}}