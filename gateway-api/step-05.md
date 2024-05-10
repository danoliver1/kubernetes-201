
Let's deploy our gateway resources!

We'll create a Gateway specifically for `purple-team` that lives in their namespace. We could instead deploy a single Gateway for the whole cluster but I like this self-service approach where teams can deploy their own isolated Gateway.

```bash
kubectl apply -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1beta1
kind: Gateway
metadata:
  name: purple-team-gateway
  namespace: purple-team
  annotations:
    networking.istio.io/service-type: ClusterIP
spec:
  gatewayClassName: istio
  listeners:
  - name: http
    port: 80
    protocol: HTTP
    allowedRoutes:
      namespaces:
        from: Same
EOF
```{{exec}}

We can wait for the Gateway to be ready with this command

```bash
kubectl wait --for=condition=programmed gtw purple-team-gateway
```{{exec}}

Due to limitations of the lab environment we must port forward

```bash
kubectl port-forward --address 0.0.0.0 service/purple-team-gateway-istio 80:80
```{{exec}}

Now access it via

[ACCESS HELLO WORLD]({{TRAFFIC_HOST1_80}})

or in another terminal tab we should be able to 

```bash
curl localhost
```{{exec}}


*The annotation `networking.istio.io/service-type: ClusterIP` sets the generated service type to `ClusterIP`. It is needed due to limitations of the lab environment. Leaving it as `LoadBalancer` is probably fine in most cases.*