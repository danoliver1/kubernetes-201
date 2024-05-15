
It is recommended that you avoid privileged containers unless unavoidable, because they have unrestricted access to the host system, potentially allowing it to perform harmful actions or access sensitive information.

There are certain scenarios where privileged access is necessary, such as a pod for monitoring host activity (runtime security). 

Let's create a namespace with the `baseline` policy and another with `restricted` to see what happens

```
kubectl create ns baseline-ns
kubectl label ns baseline-ns pod-security.kubernetes.io/enforce=baseline

kubectl create ns restricted-ns
kubectl label ns restricted-ns pod-security.kubernetes.io/enforce=restricted
```{{exec}}


And try to add a privileged pod to `baseline`

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: privileged-pod
  namespace: baseline-ns
spec:
  containers:
  - name: privileged-container
    image: alpine
    securityContext:
      privileged: true
    command: ["sleep", "3600"]
EOF
```{{exec}}

And try to add a privileged pod to `restricted`

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: privileged-pod
  namespace: restricted-ns
spec:
  containers:
  - name: privileged-container
    image: alpine
    securityContext:
      privileged: true
    command: ["sleep", "3600"]
EOF
```{{exec}}