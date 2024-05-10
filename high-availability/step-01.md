
We're going to create a `Deployment` for our workload, but firstly lets deploy a `Pod` and see the limitations of it

#### Pod

```bash
kubectl run hello-world --image httpd
```{{exec}}

We can see the pod running here

```bash
kubectl get pod hello-world
```{{exec}}

We have a single replica of our workload, which isn't highly available.

Additionally, we will lose access to our workload if the Kubernetes node is terminated, the container exits with an error or the pod is accidentally deleted. 

```bash
kubectl delete pod hello-world --now
kubectl get pod
```{{exec}}

#### Deployment

A better option is to use a `Deployment`

```bash
kubectl create deployment hello-world --image httpd
```{{exec}}

After a few moments, we should see the deployment showing as available

```bash
kubectl get deployments
```{{exec}}

And a pod is automatically created

```bash
kubectl get pods
```{{exec}}

When using a `Deployment`, the pods are self-healing. This means that if a pod terminates, it will automatically be recreated.

If a Kubernetes node is terminated, for example for upgrades, the pod will be terminated then rescheduled onto a new node.

We can test this

- Delete the pod using `kubectl delete pod -l app=hello-world --now`{{exec}}

- Run `kubectl get pods`{{exec}} again and notice that a new pod has been created with a different name


Tidy up...

```bash
kubectl delete deployment hello-world
```{{exec}}

The pods will be cleaned up automatically. To check run

```bash
kubectl get pods
```{{exec}}