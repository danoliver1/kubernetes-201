
<br>

### Welcome!

In this scenario we'll learn how to apply [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/), scoped to namespaces and the whole cluster.

These policies are built into Kubernetes and can be enabled very easily.

We will look at the three policies below in more detail

- **Privileged:** Unrestricted policy, providing the widest possible level of permissions. This policy allows for known privilege escalations.

- **Baseline:**	Minimally restrictive policy which prevents known privilege escalations. Allows the default (minimally specified) Pod configuration.

- **Restricted:** Heavily restricted policy, following current Pod hardening best practices.