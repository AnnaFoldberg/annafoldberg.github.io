---
title: "Idempotent Operations"
categories: [microservices, kubernetes]
tags: [idempotency, reliability, patterns, best-practices]
lang: en
locale: en
nav_order: 4
ref: note-idempotent-operations
---
An idempotent operation is one that has the same effect no matter how many times you perform it ⇒ the final state will always be the same.  

##### Idempotent by design
- **GET:** Always returns the same info.  
- **DELETE:** Once deleted, repeating the call doesn’t delete it more.  
- **PUT:** Setting the resource state repeatedly keeps it the same.  

##### Not idempotent by design
- **POST:** By default, POST creates a new resource or triggers an action.  

##### How to make non-idempotent operations idempotent
- **Idempotency key:** If retries must be safe (e.g., payments or order submission), you can make a POST idempotent by requiring a unique idempotency key.  
    - Client sends a unique key with the request.  
    - Server ensures the same key = same result.  
- **Upserts instead of inserts:** Use PUT instead of POST. PUT sets the full state, so if it’s repeated, it still reaches the same final state.  
- **State checks:** Before writing, check if it’s already done.  

##### What should be idempotent in microservices
- **Mutating operations that may be retried:** If the network fails, clients/Kubernetes may retry automatically. With message queues, brokers may also redeliver. If the operation is not idempotent, retries can lead to double booking, double charging, or duplicated jobs.  
- **Infrastructure tasks:** Reapplying the same configuration (e.g. Kubernetes manifest) should leave the system in the same state without causing errors.  

##### What does not have to be idempotent
- **POST operations when duplicates are intentional business actions:** E.g., writing to an audit log, creating chat messages, liking a post, etc. In these cases, calling twice should create two entries.