

Here we will deploy our Redis cache. We need some Redis config, the Redis pod and a service so other pods can access it at `redis:6379`

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: ConfigMap
metadata:
  name: redis-config
data:
  redis.conf: |-
    maxmemory 128MB
    maxmemory-policy allkeys-lru
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:7.4.0
        args: ["/redis/etc/redis.conf"]
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: 50m
            memory: 128Mi
          limits:
            cpu: 2
            memory: 128Mi
        volumeMounts:
        - name: redis-config
          mountPath: /redis/etc
      volumes:
      - name: redis-config
        configMap:
          name: redis-config
---
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  ports:
  - port: 6379
  selector:
    app: redis
EOF
```{{exec}}

After running this we should see our pod 

```bash
kubectl get pods
```{{exec}}