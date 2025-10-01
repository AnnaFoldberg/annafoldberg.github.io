---
title: "Container Image Security"
categories: [microservices, it-security]
tags: [containers, image-security]
lang: en
locale: en
nav_order: 32
ref: note-container-image-security
---
Container images are built from Dockerfiles, which define the instructions for adding and configuring a microservice. Each instruction adds a new layer to the image, starting from a base image, often provided by trusted sources like Microsoft, Red Hat, or official Linux distributions. Once built, images are stored in a registry and pulled to run as containers.

Security begins with using official base images from trusted repositories. Unverified images can introduce malicious code, creating a supply chain attack that compromises production systems. Official images are routinely updated to patch vulnerabilities, so staying current is critical.

**Image maintenance practices:**

- Scan registries regularly for vulnerabilities using automated tools like Snyk  
- Rebuild images with updated base images when CVEs are discovered  
- Redeploy containers to replace outdated instances  
- Prefer thin base images to reduce the attack surface and limit attacker options  

These practices ensure that containerized microservices remain secure and resilient against evolving threats.

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>