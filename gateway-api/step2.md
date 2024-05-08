
We're going to install Istio using the Helm charts because it is nice and simple. 

1. Add the Helm repository

```bash
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
```{{exec}}

2. Install Istio

```bash
kubectl create namespace istio-system
helm install istio-base istio/base -n istio-system --set defaultRevision=default
helm install istiod istio/istiod -n istio-system --wait
```{{exec}}

3. Check that the Istio pods are showing as `Ready`

```bash
k get po -n istio-system
```{{exec}}

**Note: If there are any errors, please review the [Helm install guide](https://istio.io/latest/docs/setup/install/helm/) and [Contact Us](#) if unable to resolve.**
