

With our **Cluster Operator** hat on, we're going to install Istio using the Helm charts because it is nice and simple. 

Add the Helm repository

```bash
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
```{{exec}}

Install two versions of Istio. This can take a minute or two, **please be patient**.

```bash
kubectl create namespace istio-system

helm install istio-base istio/base -n istio-system --set defaultRevision=default

helm upgrade istiod-1-23-0 istio/istiod -n istio-system \
    --install \
    --version 1.23.0 \
    --set revision=1-23-0 \
    --set revisionTags={default} \
    --set pilot.resources.requests.memory=512Mi \
    --set pilot.resources.requests.cpu=50m \
    --wait

helm upgrade istiod-1-23-1 istio/istiod -n istio-system \
    --install \
    --version 1.23.1 \
    --set revision=1-23-1 \
    --set pilot.resources.requests.memory=512Mi \
    --set pilot.resources.requests.cpu=50m \
    --wait
```{{exec}}

Check that the Istiod pod is showing as `Running`

```bash
kubectl get pods -n istio-system
```{{exec}}

*Note: If there are any errors, please review the [Helm install guide](https://istio.io/latest/docs/setup/install/helm/) and [Contact Us](#) if unable to resolve*
