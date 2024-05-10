
Our `Deployment` self-heals and is highly available because it has multiple replicas.

We can further improve availability by adding a `Pod Disruption Budget` to ensure that Kubernetes is only allowed to terminate a certain number or percentage of pods at a time. 

Without a `Pod Disruption Budget`, an involuntary disruption like node maintenance could result in all of the pods being terminated at the same time (while they are being rescheduled to new nodes).

```bash
kubectl apply -f - <<EOF
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: hello-world
spec:
  maxUnavailable: 1
  selector:
    matchLabels:
      app: hello-world
EOF
```{{exec}}

Now if we try deleting all of our pods at the same time, we should see that only one will restart at a time.

```bash
kubectl delete pods -l app=hello-world
```{{exec}}

The containers restart so quickly that we can't really see what's happening. This is a major benefit of running workloads in Kubernetes.

Here is a very similar Deployment but it restarts a bit slower so we can see what is happening.

```bash
kubectl apply -f foo.yaml
kubectl wait --for=condition=available deploy foo -n foo
```

Now try deleting the pods again

```bash
kubectl delete pods -l app=foo -n foo
```
