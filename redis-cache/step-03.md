

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
        livenessProbe:
          exec:
            command:
            - redis-cli
            - ping
          initialDelaySeconds: 30
          periodSeconds: 15
        readinessProbe:
          exec:
            command:
            - redis-cli
            - ping
          initialDelaySeconds: 5
          periodSeconds: 10
        securityContext:
          runAsNonRoot: true
          runAsUser: 1001
          runAsGroup: 1001
        volumeMounts:
        - name: redis-config
          mountPath: /redis/etc
      - name: redis-exporter
        image: oliver006/redis_exporter:v1.62.0
        ports:
        - containerPort: 9121
        args:
          - "--redis.addr=redis://localhost:6379"
        resources:
          requests:
            cpu: 10m
            memory: 64Mi
          limits:
            cpu: 50m
            memory: 64Mi
        securityContext:
          runAsNonRoot: true
          runAsUser: 1002
          runAsGroup: 1002
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