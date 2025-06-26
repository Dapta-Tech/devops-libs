# 🚀 Master GitOps: Deploy Helm Charts with ArgoCD & Flux Like a Pro!

To deploy this example you need to install Flux first with Helm Chart where you can apply this manifest to your cluster to deploy this example.

``` yaml
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: base-infrastructure-repo
  namespace: flux-system
spec:
  url: https://github.com/Dapta-Tech/devops-libs
  ref:
    branch: main
  interval: 1h
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: base-infrastructure
  namespace: flux-system
spec:
  sourceRef:
    kind: GitRepository
    name: base-infrastructure-repo
  path: ./gitops/deploy-argo-with-flux-and-helm/flux-base-infra
  interval: 1h
  prune: true
```

## ArgoCD Hello World App

``` yaml
apiVersion: v1
kind: Secret
metadata:
  name: pnkcore-git-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: https://github.com/Dapta-Tech/devops-libs
  project: default
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: hello-world-app
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: ./gitops/deploy-argo-with-flux-and-helm/argocd-hello-world-app
    repoURL: https://github.com/Dapta-Tech/devops-libs
    targetRevision: HEAD
```

## ArgoCD - Flux Helm Chart Operator

``` yaml
apiVersion: v1
kind: Secret
metadata:
  name: pnkcore-git-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: https://github.com/Dapta-Tech/devops-libs
  project: default
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: flux-helm-haproxy
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: ./gitops/deploy-argo-with-flux-and-helm/flux-helm-chart-operator
    repoURL: https://github.com/Dapta-Tech/devops-libs
    targetRevision: HEAD
```