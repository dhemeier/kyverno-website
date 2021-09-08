---
title: "Add imagePullSecrets"
category: Sample
version: 
subject: Pod
policyType: "mutate"
description: >
    Images coming from certain registries require authentication in order to pull them, and the kubelet uses this information in the form of an imagePullSecret to pull  those images on behalf of your Pod. This policy searches for images coming from a registry called `corp.reg.com` and, if found, will mutate the Pod to add an imagePullSecret called `my-secret`.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/add-imagepullsecrets.yaml" target="-blank">/other/add-imagepullsecrets.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-imagepullsecrets
  annotations:
    policies.kyverno.io/title: Add imagePullSecrets
    policies.kyverno.io/category: Sample
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      Images coming from certain registries require authentication in order to pull them,
      and the kubelet uses this information in the form of an imagePullSecret to pull 
      those images on behalf of your Pod. This policy searches for images coming from a
      registry called `corp.reg.com` and, if found, will mutate the Pod to add an
      imagePullSecret called `my-secret`.
spec:
  background: false
  rules:
  - name: add-imagepullsecret
    match:
      resources:
        kinds:
        - Pod
    mutate:
      patchStrategicMerge:
        spec:
          containers:
          - (image): "corp.reg.com/*"
          imagePullSecrets:
          - name: my-secret
```