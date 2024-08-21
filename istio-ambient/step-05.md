

With a standard Istio install our `web` pod would have two containers - the nginx container and an Istio sidecar. 

If we look at our web server pod, we can see that it only has one container. 

```
kubectl get pods -n webapp -l app=web -o json | jq .items[0].spec.containers
```{{exec}}

We can confirm that it is definitely "Istio enabled" by creating a temporary curl pod in a non-Istio enabled namespace and trying to make a request to it.

```bash
kubectl run --rm -it --image curlimages/curl from-a-non-istio-namespace --restart=Never -- web.webapp
```{{exec}}

This request will fail. The `default` namespace doesn't have the Istio label so pods created in the `default` namespace are not part of the Istio mesh. We added a resource to enforce "mTLS STRICT" and therefore only pods that are part of the mesh, with mTLS encryption, can make requests to our web server.