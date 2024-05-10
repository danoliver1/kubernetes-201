
Finally, with our **Development Team** hat still on, lets configure the Gateway so that when we go to our website `http://{domain}/`, the requests will be sent to our `hello-world` service!

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
EOF
```{{exec}}

That's it. Amazing. Good job!

Let's check it works.

Due to limitations of the lab environment we must port forward

```bash
kubectl port-forward --address 0.0.0.0 service/purple-team-gateway-istio 80:80
```{{exec}}

Now access it via the public URL

{{TRAFFIC_HOST1_80}}


We can also open a new Terminal tab (at the top) and run 

```bash
curl localhost
```{{exec}}

or 

```bash
curl TRAFFIC_HOST1_80
```{{exec}}