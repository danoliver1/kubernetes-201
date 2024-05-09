
Finally, lets configure the Gateway so that when we go to our website `http://{domain}/`, the requests will be sent to our `hello-world` service!

```bash
kubectl apply -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1beta1
kind: HTTPRoute
metadata:
  name: hello-world-route
  namespace: purple-team
spec:
  parentRefs:
  - name: purple-team-gateway
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /
    backendRefs:
    - name: hello-world
      port: 80
```{{exec}}

That's it. Amazing. Good job!

Let's check it works.

```bash
curl {{TRAFFIC_HOST1_80}}
```{{exec}}

or

[Open in a tab]({{TRAFFIC_HOST1_80}})