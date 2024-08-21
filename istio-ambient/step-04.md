

This is an example workload. In this case it's a basic nginx web server that returns a response when we call it with `GET http://web/`

```bash
kubectl apply -n webapp -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: web
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: web
  name: web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      serviceAccountName: web
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: web
  name: web
  namespace: webapp
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: web
EOF
```{{exec}}

We'll also create another pod to make requests and test connectivity to the web server

```bash
kubectl apply -n webapp -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: curl
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: curl
  name: curl
  namespace: webapp
spec:
  serviceAccountName: curl
  containers:
  - command:
    - sh
    - -c
    - sleep infinity
    image: curlimages/curl
    name: curl
EOF
```{{exec}}

Note: we also created and used a service account named "curl" which we will later use in an authz policy.