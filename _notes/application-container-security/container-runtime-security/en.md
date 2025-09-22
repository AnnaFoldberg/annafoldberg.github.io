---
title: "Container Runtime Security"
categories: [microservices, it-security]
tags: [containers, runtime-security]
lang: en
locale: en
nav_order: 31
ref: note-container-runtime-security
---
To receive the full benefits of a microservice architecture, it takes a flexible execution environment that allows for rapid yet stable deployments. This is why the majority of developers choose to deploy microservices inside of containers.

Containers have their own attack surface and expose new attack vectors. When running microservices on containers, security measures can be applied to harden various components within the architecture, including the host and the container runtime.

**Host security:**

- Keep access limited (if using Kubernetes, this number should be even smaller, because the need to access the host is diminished)  
- Use a distro designed to run container workloads (e.g. Red Hat’s Fedora CoreOS)  
  - These distros reduce the attack surface on the host by removing non-essential applications. This lowers the number of vulnerabilities found within the operating system.  
- Patch and harden similar to other virtual machines  

**Secure container runtime configurations**

- **Least privilege:** Configure containers to run with the least amount of privileges necessary. An effective way to achieve this is to drop all the kernel capabilities initially, and then add in only those capabilities that are necessary to run the container.  
- **Privilege escalation (no-new-privileges flag):** Prevents containers from escalating their privileges by explicitly denying it in the configuration (in other words: prevents the process inside the container from gaining new privileges during execution).  
- **Privileged mode (privileged flag):** Never run containers using the `--privileged` flag. It significantly increases the risk of container breakout.  
- **Set a user:** Configure the container to run as an unprivileged user to prevent escalation attacks.  
  - Set the user that will be used when running the image. This is done either with the image configuration or when the container is launched.  
  - If a user is not set, your container will run as root by default, and root inside the container is root on the host. If an attacker manages to escape the container, they will have complete access to the underlying host.  
- **Read only:** Configure containers with read-only file systems and mount volumes as read-only.  

**Docker Bench:** Scans running containers and reports on any configurations that deviate from benchmark industry security standards.  

- Script to check for common best practices in deployed containers.  
- Conducts tests in accordance with CIS Docker Benchmark.  
- Open-source and easy to run.  
- [Docker Bench Security](https://github.com/docker/docker-bench-security)  

**Best practices:**

- Automate detection of configurations that deviate from secure practices  
- Enforce secure configurations in CI/CD  
- Establish templates for secure container orchestrator configuration  

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>