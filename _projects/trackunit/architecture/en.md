---
title: "System Architecture"
categories: [trackunit, microservices, kubernetes, it-security]
tags: [overview, architecture]
lang: en
locale: en
ref: project-trackunit-architecture
---
![Architecture diagram](../../../assets/images/architecture.png)
We see our project as having three main processes, each with a specific call flow:

##### Image Upload
Client → API Gateway → Upload Service (create pre-signed URL)  
Client → Object storage (store image)  
Client → API Gateway → Upload Service → Store metadata about image in its own database

##### Image Analysis
Client → API Gateway → Analysis Service (orchestrates the workflow)  
Analysis Service → Upload Service (verify upload, get URL)  
Analysis Service → AI Image Service (infer on image URL)  
Analysis Service → AI Metadata Service (look up further metadata)  
Analysis Service → Metadata Service (match information to attributes)  
Analysis Service → Machine Service (store machine metadata in database)  
Analysis Service → store analysis result in its own database  
→ API Gateway → Client

#### Read Existing Data
Client → API Gateway → Machine Service (owns the machine database, provides CRUD operations)  
→ API Gateway → Client