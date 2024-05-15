
We should aim to enforce the `restricted` rules where possible, falling back to `baseline` when this is not possible.

It is recommended to avoid running privileged containers unless unavoidable, because they have unrestricted access to the host system, potentially allowing it to perform harmful actions or access sensitive information. It's probably a good idea to set the whole cluster to default to at least `baseline` and then force teams to explicitly set namespaces to `privileged` if it's completely necessary for them to run privileged workloads.

Let's create a namespace with the `baseline` policy and another with `restricted` to see what happens when we try to run a pod with `privileged: true`

```
kubectl create ns baseline-ns
kubectl label ns baseline-ns \
  pod-security.kubernetes.io/enforce=baseline

kubectl create ns restricted-ns
kubectl label ns restricted-ns \
  pod-security.kubernetes.io/enforce=restricted
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

Both should fail and prevent privileged containers from running.