
We'll create a simple deployment and a service to expose it

```bash
kubectl -n purple-team create deployment hello-world --image nginx
kubectl -n purple-team expose deployment hello-world --port 8080
```{{exec}}

Let's have a look at what we've created:

We have a Deployment

```bash
kubectl get deployment -n purple-team
```{{exec}}

Which has created our pod

```bash
kubectl get po -n purple-team
```{{exec}}

And an internal "ClusterIP" service which has exposed our pod interally on port 8080

```bash
kubectl get service -n purple-team
```{{exec}}

We can now test that our service is accessible by running curl from a temporary pod

```bash
kubectl run curl --image curlimages/curl --restart=Never --rm -it -- hello-world:8080
```

*The above command creates a pod using the `curlimages/curl` docker image, runs the command `curl hello-world:8080` then removes the pod after the command has completed.*