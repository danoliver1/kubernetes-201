
Our `Deployment` self-heals and is highly available because it has multiple replicas.

We can further improve availability by adding a `Pod Disruption Budget` to ensure that Kubernetes is only allowed to terminate a certain number or percentage of pods at a time. 

Without a `Pod Disruption Budget`, an involuntary disruption like node maintenance could result in all of the pods being terminated at the same time (while they are being rescheduled to new nodes).

```bash
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: hello-world
spec:
  maxUnavailable: 1
  selector:
    matchLabels:
      run: hello-world
```{{exec}}

Now if we try deleting all of our pods at the same time, we should see that only one will restart at a time.

```bash
kubectl delete pods -l run=hello-world
```{{exec}}