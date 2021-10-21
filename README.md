# RedHat Advanced Cluster Manager for Kubernetes policy examples
This repo contains examples how to work with RHACM policy examples and kustomize

# How to implement
Create a Channel and Subscription, for example:

```
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  annotations:
    apps.open-cluster-management.io/reconcile-rate: high
  name: prod-policies-chan
  namespace: policies
spec:
  pathname: https://github.com/RobMokkink/RHACM-policy-examples.git
  type: GitHub
---
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  annotations:
    apps.open-cluster-management.io/git-branch: main
    apps.open-cluster-management.io/git-path: overlays/prod
  name: prod-policies-sub
  namespace: policies
spec:
  allow:
  - apiVersion: policy.open-cluster-management.io/v1
    kinds:
    - '*'
  - apiVersion: cluster.open-cluster-management.io/v1alpha1
    kinds:
    - '*'
  channel: policies/prod-policies-chan
  placement:
    local: true
```

This information can be found in upstream repo https://github.com/open-cluster-management/policy-collection, folder deploy

You can create another one for your dev branch and adjust the overlay accordingly, see the following example:

```
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  annotations:
    apps.open-cluster-management.io/reconcile-rate: high
  name: dev-policies-chan
  namespace: policies
spec:
  pathname: https://github.com/RobMokkink/RHACM-policy-examples.git
  type: GitHub
---
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  annotations:
    apps.open-cluster-management.io/git-branch: dev
    apps.open-cluster-management.io/git-path: overlays/dev
  name: dev-policies-sub
  namespace: policies
spec:
  allow:
  - apiVersion: policy.open-cluster-management.io/v1
    kinds:
    - '*'
  - apiVersion: cluster.open-cluster-management.io/v1alpha1
    kinds:
    - '*'
  channel: policies/dev-policies-chan
  placement:
    local: true
```
