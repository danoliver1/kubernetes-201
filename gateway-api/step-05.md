
With our **Development Team** hat on, we'll create a simple deployment and a service to expose it so we can access it.

Please note that using the `kubectl` cli is not how I'd recommend deploying production-grade apps, but it's a nice quick way that we can use for testing. Productionising Kubernetes is out of scope for this course.

```bash
kubectl create deployment hello-world --image httpd
kubectl expose deployment hello-world --port 80
kubectl wait --for=condition=available deploy hello-world
```{{exec}}

Let's have a look at what we've created:

We have a Deployment

```bash
kubectl get deployments
```{{exec}}

Which has created our pod

```bash
kubectl get pods
```{{exec}}

And an internal "ClusterIP" service which has exposed our pod interally on port 8080

```bash
kubectl get services
```{{exec}}

Here we will create a `curl` pod that we can use to run curl within the cluster.

```bash
kubectl run curl --image curlimages/curl --command -- sh -c "sleep infinity"
```{{exec}}

We can now test our service is accessible internally using
```bash
kubectl exec curl -- curl hello-world
```{{exec}}

The response body should be `<html><body><h1>It works!</h1></body></html>` 

Lets tidy up

```bash
kubectl delete pod curl --now
```{{exec}}