

This scenario will explain how to prevent 503s during pod termination by adding a pre-stop delay.

When a pod is killed, a SIGTERM signal is sent to the web server running in the pod and it will usually stop accepting new requests immediately. Most web servers will finish serving the requests that are in progress before shutting down.

Unfortunately the because the web server stops accepting requests immediately, this can result in 503s while the pod's endpoint is being removed from the Service (k8s load balancer). 

A simple fix is to add a slight delay before shutting down the pod to make sure the Service is updated before SIGTERM is sent to the web server.

This can be added to the pod spec under `.spec.containers[]`

```yaml
      lifecycle:
        preStop:
          exec:
            command: ["sleep", "15"]
```

This fix requires the container to have the `sleep` binary present, but it is language agnostic.

It might be worth checking the documentation of the web server to see if there is a parameter that can be set to add a delay before reacting to the SIGTERM.
