
Add the Helm repository
```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
```{{exec}}

Firstly, we'll install Jenkins with the default configuration to see what is there
```
helm upgrade --install jenkins jenkins/jenkins
```{{exec}}

This takes a little while to finish, so let's keep an eye on it with this command
```bash
watch "kubectl get all"
```{{exec}}

*Press `CTRL+C` to stop the watch after the pods show as Running.*

Now we can get the admin password

```bash
kubectl exec -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
```{{exec}}

And forward port 8080 so we can access the UI

```bash
kubectl port-forward --address 0.0.0.0 service/jenkins 8080:8080
```{{exec}}

We should now be able to access:

[JENKINS UI]({{TRAFFIC_HOST1_8080}})

Use the username `admin` and password from the previous step to login and explore.

*Press `CTRL+C` to stop the port forwarding.*