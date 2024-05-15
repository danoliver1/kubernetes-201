
Let's create a pod which meets the `restricted` standards.


If we start with a very simple pod spec, this should get rejected with an error message showing what we need to change.


```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: restricted-pod
  namespace: restricted-ns
spec:
  containers:
  - name: container
    image: alpine
    command:
    - sh
    - -c
    - "sleep infinity"
EOF
```{{exec}}

**We will work through these errors, fixing them one at a time.**

Fix `must set securityContext.allowPrivilegeEscalation=false`

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: restricted-pod
  namespace: restricted-ns
spec:
  containers:
  - name: container
    image: alpine
    command:
    - sh
    - -c
    - "sleep infinity"
    securityContext:
      allowPrivilegeEscalation: false
EOF
```{{exec}}

Fix `must set securityContext.capabilities.drop=["ALL"]`

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: restricted-pod
  namespace: restricted-ns
spec:
  containers:
  - name: container
    image: alpine
    command:
    - sh
    - -c
    - "sleep infinity"
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
EOF
```{{exec}}

Fix `must set securityContext.runAsNonRoot=true`

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: restricted-pod
  namespace: restricted-ns
spec:
  containers:
  - name: container
    image: alpine
    command:
    - sh
    - -c
    - "sleep infinity"
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
      runAsNonRoot: true
EOF
```{{exec}}

Fix `must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost"`

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: restricted-pod
  namespace: restricted-ns
spec:
  containers:
  - name: container
    image: alpine
    command:
    - sh
    - -c
    - "sleep infinity"
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
      runAsNonRoot: true
      seccompProfile:
        type: RuntimeDefault
EOF
```{{exec}}

The pod should now pass the `restricted` rules and have deployed successfully.

**This pod is "following current Pod hardening best practices"**