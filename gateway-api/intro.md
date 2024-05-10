
<br>

### Welcome !

In this scenario we'll learn how to use the [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/) with [Istio Service Mesh](https://istio.io).

*This guide assumes prior knowledge of Istio. If you are not familiar with Gateway API or Istio then please follow the links above before continuing.*

#### Why the Gateway API is awesome!

Before the Gateway API existed, we had to deploy Istio Ingress Gateways manually and use vendor specific resources. This meant that **Platform Engineering** teams had to deploy the Istio Ingress Gateway Helm chart as part of the namespace provisioning process. **Development Teams** had to tightly-couple their application with Istio, and configure routing using Istio's complicated Virtual Service resources.

With the Gateway API, **Platform Engineering** can provision a Gateway very easily with a `Gateway` resource and **Development teams** can configure routing with very simple `HttpRoute` resources which are vendor neutral.

So the Gateway API allows different teams to effictively manage their own resources.

For example, the Gateway API would work well with this structure

 - **Cluster Operators:** responsible for setting up and maintaining Istio and the Gateway CRDs.

 - **Platform Engineers:** responsible for creating the namespaces and Gateways, or providing a self-service method to provision these.

 - **Application Developer:** responsible for creating and deploying their application and defining the HTTP routes.
