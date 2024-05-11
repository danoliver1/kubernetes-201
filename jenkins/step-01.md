
Add the Helm repository
```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
```{{exec}}


Firstly, we'll install Jenkins with the default configuration to see what is there
```
helm upgrade --install jenkins jenkins/jenkins
```{{exec}}