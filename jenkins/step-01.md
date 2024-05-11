
Add the Helm repository
```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
```{{exec}}


Firstly, we'll install Jenkins with the default configuration to see what is there
```
helm upgrade --install jenkins jenkins/jenkins
```{{exec}}

This takes a little while to finish, so lets keep an eye on it with this command
```bash
watch "kubectl get all"
```{{exec}}

Press `CTRL+C` to stop the watch after the pods show as Running.