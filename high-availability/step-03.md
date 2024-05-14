
Even though our `Deployment` self-heals and is highly available because it has multiple replicas, unfortunately we might still experience downtime when there are involuntary disruptions, like node upgrades.

As an example, if the cluster administrator was to upgrade all of the cluster's nodes, by default all of your workload's pods can be evicted (terminated) at the same time.

By creating a `Pod Disruption Budget` we can configure Kubernetes to only evict a certain number or percentage of pods at a time. 

So without a pod disruption budget, an involuntary disruption like node maintenance could result in momentary downtime, due to all of the pods terminating at the same time.

For our example workload, this pod disruption budget that will only allow one pod to be evicted at a time

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

Please note that a pdb does not prevent us from manually deleting all of the pods at the same time

```bash
kubectl delete pods -l app=hello-world
```{{exec}}

We can test the pod disruption budget by simulating node maintenance and making the node unschedulable.

```bash
kubectl drain controlplane --ignore-daemonsets
```{{exec}}

We should see an error message letting us know that it cannot evict all of our pods.

Finally uncordon the node to make if schedulable again.

```bash
kubectl uncordon controlplane
```{{exec}}