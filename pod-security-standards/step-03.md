
Create an admission configuration file specifying the defaults

```bash
cat <<EOF > /etc/kubernetes/admission-configuration.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
- name: PodSecurity
  configuration:
    apiVersion: pod-security.admission.config.k8s.io/v1
    kind: PodSecurityConfiguration
    defaults:
      enforce: "baseline"
      enforce-version: "latest"
      audit: "restricted"
      audit-version: "latest"
      warn: "restricted"
      warn-version: "latest"
    exemptions:
      usernames: []
      runtimeClasses: []
      namespaces: [kube-system]
EOF
```{{exec}}


And update the `api-server` arguments to include

```bash
--admission-control-config-file: /etc/kubernetes/admission-configuration.yaml
```

We can try this by adding the above flag to the arguments list in
```bash
vi /etc/kubernetes/manifests/kube-apiserver.yaml
```{{exec}}