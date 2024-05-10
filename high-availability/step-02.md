
With a `Deployment` the pods self-heal when terminated. This will still result in momentary downtime as the pod restarts.

We can improve on this by running multiple replicas of a pod.

This is very simple in Kubernetes

```bash
kubectl create deployment hello-world --replicas 3
```{{exec}}

If we wait a few moments, we should see three pods appearing.

```bash
kubectl get pods
```{{exec}}


Now if one pod restarts, there will still be another two pods available to serve requests.

If you delete pods using the below command, you'll notice that they self-heal and we will have either two or three pods running at all times

```bash
kubectl delete pod POD_NAME --now
```

This should not result in any down time because when the pod is being deleted, two other pods are still running.

*Note: EKS is configured so that by default, the pods are scheduled across multiple availability zones. This is something that happens automatically without any extra configuration.*