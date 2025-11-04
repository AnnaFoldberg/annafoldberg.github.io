---
title: "Trackunit: GraphQL Schema (SDL)"
categories: [trackunit, graphql]
tags: [graphql]
lang: da
locale: da
nav_order: 17
ref: graphql-schema
---

```graphql
schema {
  query: Query
  mutation: Mutation
  subscription: Subscription
}

type AnalysisCompleted {
  correlationId: String!
  objectKey: String!
  success: Boolean!
}

type AnalysisRequestPayload {
  correlationId: String!
}

type AnalysisStarted {
  correlationId: String!
  objectKey: String!
}

type Mutation {
  requestAnalysis(input: AnalysisRequestInput!): AnalysisRequestPayload!
    @authorize(policy: "RequireApiScope")
    @cost(weight: "10")
}

type Query {
  ping: String!
}

type Subscription {
  onAnalysisStarted: AnalysisStarted! @authorize(policy: "RequireApiScope")
  onAnalysisCompleted: AnalysisCompleted! @authorize(policy: "RequireApiScope")
}

input AnalysisRequestInput {
  objectKey: String!
}