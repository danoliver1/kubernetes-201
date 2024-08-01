

Deploy a simple web application with 3 replicas that returns the hostname of the pod

```bash
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: web
  name: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
        sidecar.istio.io/inject: "true"
    spec:
      initContainers:
      - image: alpine
        name: init
        command: ['sh', '-c', 'echo "\${HOSTNAME}" > /usr/local/apache2/htdocs/index.html']
        volumeMounts:
        - name: web
          mountPath: /usr/local/apache2/htdocs
      containers:
      - image: httpd
        name: httpd
        ports:
        - containerPort: 80
        volumeMounts:
        - name: web
          mountPath: /usr/local/apache2/htdocs
      volumes:
      - name: web
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: web
  name: web
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: web
  type: ClusterIP
EOF
```{{exec}}

Configure the routing

```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1
kind: VirtualService
metadata:
  name: web
spec:
  gateways:
  - sticky-session-test-gateway
  hosts:
  - "*"
  http:
  - route:
    - destination:
        host: web
        port:
          number: 80
EOF
```{{exec}}


We have now deployed our sample app and made it available at route `/`

Let's check it works.

Due to limitations of the lab environment we must port forward

```bash
kubectl port-forward --address 0.0.0.0 service/sticky-session-test-gateway 80:80
```{{exec}}

Now access it via the public URL

{{TRAFFIC_HOST1_80}}
