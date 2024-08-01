

For testing purposes, we'll deploy a simple web application with 5 replicas that returns the name of the pod

```bash
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: web
  name: web
spec:
  replicas: 5
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
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

**Please leave this port forward running** and **open another tab** in the Terminal by clicking the `+` icon at the top of the window.