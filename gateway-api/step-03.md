
With our **Platform Engineering** hat on, lets create a namespace for the **Development Team** to put their example app in.

```bash
kubectl create ns purple-team
```{{exec}}

We want our pods to be "Istio enabled" so we must label the namespace. 

```bash
kubectl label namespace purple-team istio-injection=enabled
```{{exec}}

This label will make sure our pods have an Istio sidecar.

Also we'll set the default namespace to be `purple-team` so we don't have to add `-n purple-team` to all of our commands

```bash
kubectl config set-context --current --namespace=purple-team
```{{exec}}

*This whole step might be completed by **Development Team** as part of a self-service template. For example using [Backstage Developer portal](https://backstage.io/). The templates are created by a **Platform Engineering Team***