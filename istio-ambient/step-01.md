

Add the Helm repository.

```bash
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
```{{exec}}

Install Istio. This can take a minute or two, **please be patient**.

```bash
kubectl create namespace istio-system

helm install istio-base istio/base -n istio-system \
    --set defaultRevision=default

helm install istiod istio/istiod -n istio-system \
    --set pilot.resources.requests.memory=1Gi \
    --set global.proxy.resources.requests.cpu=10m \
    --set meshConfig.accessLogFile=/dev/stdout \
    --set profile=ambient \
    --wait
```{{exec}}

Note: to enable Ambient mode we have set `profile=ambient` for `istiod`

There are two additional components we must install for Istio Ambient mode.

The [CNI plugin](https://github.com/istio/istio/tree/master/cni) is responsible for listening to pod events and pushing networking config out to other Istio enabled pods. It also watches for enabled namespaces and will enroll pods if needed. 

```bash
helm upgrade istio-cni istio/cni -n istio-system \
    --install \
    --set profile=ambient \
    --wait
```{{exec}}

[Ztunnel](https://github.com/istio/ztunnel) is the L4 node proxy used in Ambient mesh. It is deployed as a DaemonSet so there will be one Ztunnel pod per node.

```bash
helm upgrade ztunnel istio/ztunnel -n istio-system \
    --install \
    --wait
```{{exec}}

In summary, the key difference between a standard Istio installation and Istio Ambient mode is the inclusion of profile=ambient in the istiod Helm release, along with the additional Helm releases for ztunnel and istio-cni.

Before moving on, heck all of the Istio pods are showing as `Running`

```bash
kubectl get pods -n istio-system
```{{exec}}

*Note: If there are any errors, please review the [Helm install guide](https://istio.io/latest/docs/setup/install/helm/) and [Contact Us](#) if unable to resolve*
