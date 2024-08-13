

To test the Redis cache, we will deploy another pod called `redis-client` and then run commands from that pod. This step is optional but demonstrates the connectivity.

```bash
apiVersion: v1
kind: ServiceAccount
metadata:
  name: redis-client
automountServiceAccountToken: false
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-client
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-client
  template:
    metadata:
      labels:
        app: redis-client
    spec:
      serviceAccountName: redis-client
      containers:
      - name: redis-client
        image: redis:7.4.0
        command: ["sh", "-c", "sleep infinity"]
```{{exec}}

We can then connect to the pod and run the `redis-cli`

```bash
kubectl exec deployment/redis-client -n redis-test -it -- redis-cli -h redis
```{{exec}}

Set a value in the redis cache

```bash
SET user1.name fred
```{{exec}}

Get a value from the redis cache

```bash
GET user1.name fred
```{{exec}}

Press `CTRL+C` to exit.

Your own application can connect to the redis cache in a similar way using the host `redis`