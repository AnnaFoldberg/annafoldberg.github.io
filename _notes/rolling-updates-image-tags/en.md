---
title: "Rolling Updates and Image Tags"
categories: [kubernetes]
tags: [rolling-updates, deployments, images, versioning]
lang: en
locale: en
nav_order: 62
ref: note-rolling-updates-image-tags
---

##### Rolling Updates and Image Tags

Kubernetes does not require version numbers on container images to perform rolling updates. However, if you only use `:latest`, Kubernetes will not automatically detect a new version unless the Deployment manifest changes.

##### Rollout Conditions

Kubernetes triggers a new rollout when the Pod template spec inside the Deployment changes. This happens, for example, when the image tag changes:

```yaml
image: myapp:1.1
```

This creates a new ReplicaSet because the manifest has changed.

If you instead continue using:

```yaml
image: myapp:latest
```

and only push a new latest image to the registry, Kubernetes will **not** perform a rollout, because the Deployment manifest remains unchanged. Existing Pods keep the version they already pulled.

##### latest and imagePullPolicy

The `:latest` tag implicitly sets `imagePullPolicy: Always`, meaning Kubernetes fetches the latest version whenever a Pod starts.

But Kubernetes does not restart Pods automatically just because a new version was pushed. To pull the updated latest image, you must modify the `spec.template` or run `kubectl rollout restart deployment <deployment>`.

##### Benefits of Versioned Tags

Although Kubernetes can technically operate without explicit versions, versioned tags provide several advantages:

- Predictable deployments: `myapp:1.2.3` always refers to the same code.  
- Kubernetes detects changes directly → clean rolling updates.  
- Rollbacks become simple because previous versions are identifiable.