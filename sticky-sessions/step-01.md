

Add the Helm repository.

```bash
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
```{{exec}}

Install Istio. This can take a minute or two, **please be patient**.

```bash
kubectl create namespace istio-system
helm install istio-base istio/base -n istio-system --set defaultRevision=default
helm install istiod istio/istiod -n istio-system --set pilot.resources.requests.memory=1Gi --set global.proxy.resources.requests.cpu=10m --wait
```{{exec}}

Check that the Istiod pod is showing as `Running`

```bash
kubectl get pods -n istio-system
```{{exec}}

*Note: If there are any errors, please review the [Helm install guide](https://istio.io/latest/docs/setup/install/helm/) and [Contact Us](#) if unable to resolve*
