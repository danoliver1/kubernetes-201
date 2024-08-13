

One final thing we should do is restrict which pods can access the cache. The below Istio [AuthorizationPolicy](https://istio.io/latest/docs/reference/config/security/authorization-policy) only allows requests from the `redis-client` pod

```bash
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: redis-access
spec:
  selector:
    matchLabels:
      app: redis
  rules:
  - from:
    - source:
        principals: ["cluster.local/ns/redis-test/sa/redis-client"]
EOF
```{{exec}}

We can test this by creating a new client

```
kubectl run bad-pod --image redis
```{{exec}}

If we try to connect to the Redis cache from our new test client we should get an error when we try to talk to the cache

```
kubectl exec bad-pod -it -- redis-cli -h redis
```{{exec}}

```
GET user1.name
exit
```{{exec}}


Our original client from earlier should still work

```
kubectl exec deployment/redis-client -it -- redis-cli -h redis
```{{exec}}

```
GET user1.name
exit
```{{exec}}