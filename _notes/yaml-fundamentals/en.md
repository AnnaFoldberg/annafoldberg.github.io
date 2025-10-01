---
title: "YAML Fundamentals"
categories: [kubernetes]
tags: [yaml, gitops, infrastructure-as-code]
lang: en
locale: en
nav_order: 39
ref: note-yaml-fundamentals
---
Infrastructure as Code and GitOps are two approaches often paired with Kubernetes. With Kubernetes, setting up infrastructure through code and deploying changes with a GitOps workflow requires saving the desired state of the cluster in files that are tracked in a source control system such as Git. The format most commonly used for these files is YAML manifests.

YAML is a data serialization language, often used for configuration files. Data serialization languages provide a standard way to represent information so that different devices, operating systems, and programming languages can easily share data. Other examples include JSON and XML. Using a serialization language makes data portable; YAML allows data to be packed up, moved somewhere else, and unpacked without needing extra steps from the developer.

The name YAML stands for “YAML Ain’t Markup Language.” It is designed to be human-readable, unlike byte code or assembly language, which provide instructions to a CPU but are difficult for people to understand. Learning a handful of YAML syntax rules makes it much easier to work with Kubernetes manifests.

##### YAML Syntax Essentials
- `---:` Three dashes on a line mark the beginning of a document. Multiple documents can exist in a single file, each starting with ---.  
- `#:` A hash at the beginning of a line denotes a comment.  
- `key: value:` YAML stores data as key-value pairs separated by a colon.  
- `- item:` Sequences (a list or array of items) use a dash followed by an item on its own line.  
- **Nested maps:** Indented key-value pairs create structured collections, such as jobs with years and titles.  

##### Best Practices
- **File extensions:** Both .yaml and .yml are valid.  
- **Indentation:** YAML is sensitive to indentation errors. Use a validator (e.g., yamlchecker.com) to check for mistakes.  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>