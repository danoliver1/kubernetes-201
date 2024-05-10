
With our **Platform Engineering** hat still on, let's deploy our gateway resources!

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


*The annotation `networking.istio.io/service-type: ClusterIP` sets the generated service type to `ClusterIP`. It is needed due to limitations of the lab environment. Leaving it as `LoadBalancer` is probably fine in most cases.*

*This step might be completed by **Development Team** as part of a self-service template. For example using [Backstage Developer portal](https://backstage.io/). The templates are created by a **Platform Engineering Team***