---
title: "Rolling Updates e Image Tags"
categories: [kubernetes]
tags: [rolling-updates, deployments, images, versioning]
lang: it
locale: it
nav_order: 62
ref: note-rolling-updates-image-tags
---

##### Rolling Updates e Image Tags

Kubernetes non richiede numeri di versione sulle immagini dei container per eseguire rolling updates. Tuttavia, se si usa solo `:latest`, Kubernetes non rileva automaticamente una nuova versione a meno che il Deployment manifest non venga modificato.

##### Condizioni di Rollout

Kubernetes avvia un nuovo rollout quando la Pod template spec del Deployment cambia. Ciò accade, ad esempio, quando l'image tag viene aggiornato:

```yaml
image: myapp:1.1
```

Questo genera un nuovo ReplicaSet perché il manifesto è cambiato.

Se invece si usa:

```yaml
image: myapp:latest
```

e si esegue solo il push di una nuova immagine latest nel registry, Kubernetes **non** avvierà un rollout, perché il Deployment manifest rimane invariato. I Pod esistenti continuano a usare la versione già scaricata.

##### latest e imagePullPolicy

Il tag `:latest` imposta implicitamente `imagePullPolicy: Always`, il che significa che Kubernetes scarica sempre l’ultima versione quando un Pod viene avviato.

Ma Kubernetes non riavvia automaticamente i Pod solo perché è stata caricata una nuova versione. Per ottenere la nuova immagine latest bisogna modificare la `spec.template` oppure eseguire `kubectl rollout restart deployment <deployment>`

##### Vantaggi dei Tag Versionati

Anche se Kubernetes può tecnicamente funzionare senza numeri di versione, i tag versionati offrono importanti vantaggi:

- Deployment prevedibili: `myapp:1.2.3` fa sempre riferimento allo stesso codice.  
- Kubernetes rileva direttamente i cambiamenti → rolling updates puliti.  
- I rollback sono semplici grazie alla chiara identificazione delle versioni precedenti.