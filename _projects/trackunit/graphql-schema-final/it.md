---
title: "Trackunit: GraphQL Schema (SDL)"
categories: [trackunit]
tags: [graphql, final]
lang: it
locale: it
nav_order: 22
ref: graphql-schema-final
---

{% raw %}
```graphql
schema {
  query: Query
  mutation: Mutation
  subscription: Subscription
}

type AIProviderDto {
  name: String!
  apiVersion: String
  featureset: [String!]!
  maxResults: Int
}

type AnalysisCompleted {
  success: Boolean!
  recognitionPayload: RecognitionCompleted
}

type AnalysisStarted {
  objectKey: String!
}

type ConfirmUploadPayload {
  status: String!
}

type ImageUploadPayload {
  uploadId: String!
  key: String!
  putUrl: String!
  expiresAt: DateTime!
}

type MachineAggregateDto {
  brand: String
  type: String
  model: String
  confidence: Float!
  isConfident: Boolean!
  typeConfidence: Float
  typeSource: String
  name: String
}

type Mutation {
  startUpload(filename: String!, contentType: String!): StartUploadPayload!
    @authorize(policy: "RequireApiScope")
    @cost(weight: "10")
  confirmUpload(
    uploadId: String!
    bytes: Int!
    checksum: String!
  ): ConfirmUploadPayload!
    @authorize(policy: "RequireApiScope")
    @cost(weight: "10")
}

type Query {
  ping: String!
}

type RecognitionCompleted {
  provider: AIProviderDto!
  aggregate: MachineAggregateDto!
}

type StartUploadPayload {
  correlationId: String!
  imageUploadPayload: ImageUploadPayload!
}

type Subscription {
  onAnalysisStarted: AnalysisStarted! @authorize(policy: "RequireApiScope")
  onAnalysisCompleted: AnalysisCompleted! @authorize(policy: "RequireApiScope")
}
```
{% endraw %}