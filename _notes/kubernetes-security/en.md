---
title: "Kubernetes Security"
categories: [it-security, kubernetes]
tags: [security-context, snyk]
lang: en
locale: en
nav_order: 54
ref: note-kubernetes-security
---
Like any internet-facing system, Kubernetes clusters are targets for attackers. Common goals include stealing data, hijacking cluster resources for cryptocurrency mining, or launching distributed denial-of-service (DDoS) attacks. While threats evolve, there are several best practices that can be applied immediately to improve cluster security.

##### Security Context
Containers should be configured with restrictive security settings to reduce risk if an attacker gains access. Key practices include:  
- Run containers as non-root users.  
- Disable privilege escalation.  
- Drop all Linux capabilities not explicitly required.  

##### Read-Only Root Filesystem
Set the container filesystem to read-only, using `readOnlyRootFilesystem: true`, to prevent attackers from modifying files or installing software inside the container.

##### Scan with Security Tools
Use tools such as **Snyk** to scan Kubernetes manifests for misconfigurations. Scans highlight issues such as writable root filesystems, missing user controls, and lack of privilege restrictions. Running scans regularly helps enforce secure configurations.

##### Regularly Update Kubernetes
Keep Kubernetes clusters up to date, especially when security patches are released. Monitor the [CVE database](https://cve.mitre.org/) for known Kubernetes vulnerabilities and apply fixes promptly.

##### Summary
Improving Kubernetes security begins with simple but effective measures: configure Pods with security contexts, enforce read-only filesystems, scan manifests with security tools, and update Kubernetes regularly to apply security patches.

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>