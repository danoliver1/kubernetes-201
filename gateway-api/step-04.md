
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

And an internal service which has exposed our pod

```bash
kubectl get service -n purple-team
```{{exec}}
