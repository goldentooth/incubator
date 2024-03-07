# GitOps

My app-of-apps pattern Argo CD GitOps repository.

This has a corresponding, permissive [AppProject](https://github.com/goldentooth/cluster/blob/main/roles/goldentooth.install_argocd_apps/tasks/projects/incubator.yaml):

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: incubator
  namespace: argocd
  finalizers:
    - resources-finalizer.argocd.argoproj.io
spec:
  description: GoldenTooth incubator project
  # Allow manifests to deploy from any Git repository.
  # This is an acceptable security risk because this is a lab environment.
  sourceRepos:
    - '*'
  destinations:
    # Prevent any resources from deploying into the kube-system namespace.
    - namespace: '!kube-system'
      server: '*'
    # Allow resources to deploy into any other namespace.
    - namespace: '*'
      server: '*'
  clusterResourceWhitelist:
    # Allow any cluster resources to deploy.
    - group: '*'
      kind: '*'
```

As my configuration of a given application matures, I'll migrate it into an ApplicationSet elsewhere.
