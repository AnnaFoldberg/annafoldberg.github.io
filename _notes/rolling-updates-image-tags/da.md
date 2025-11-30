---
title: "Rolling Updates og Image Tags"
categories: [kubernetes]
tags: [rolling-updates, deployments, images, versioning]
lang: da
locale: da
nav_order: 62
ref: note-rolling-updates-image-tags
---

##### Rolling Updates og Image Tags

Kubernetes kræver ikke versionsnumre på container-images for at udføre rolling updates. Men hvis man kun bruger `:latest`, opdager Kubernetes ikke automatisk, at der er kommet en ny version, medmindre der sker en ændring i Deployment-manifestet.

##### Rollout-betingelser

Kubernetes starter et nyt rollout, når Pod template spec i Deploymentet ændrer sig. Det sker f.eks. ved ændret image-tag:

```yaml
image: myapp:1.1
```

Her oprettes et nyt ReplicaSet, fordi manifestet er ændret.

Hvis man derimod fortsat bruger:

```yaml
image: myapp:latest
```

og man kun pusher et nyt latest-image til registry, vil Kubernetes **ikke** lave et rollout, fordi Deployment-manifestet er uændret. Eksisterende pods fortsætter med den image-version, de allerede har downloaded.

##### latest og imagePullPolicy

Tagget `:latest` medfører by default `imagePullPolicy: Always`, hvilket betyder, at Kubernetes henter den nyeste version, når en pod startes.

Men Kubernetes genstarter ikke pods automatisk, bare fordi der er uploadet en ny version i registry. For at hente det nye latest-image skal man ændre noget i `spec.template` eller udføre `kubectl rollout restart deployment <deployment>`.

##### Fordele ved versionstags

Selvom Kubernetes teknisk kan fungere uden versionsnumre, giver versionerede tags en række fordele:

- Forudsigelige deployments: `myapp:1.2.3` refererer altid til samme kode.  
- Kubernetes opdager ændringer direkte → rene rolling updates.  
- Rollbacks bliver enkle, fordi tidligere versioner kan identificeres.