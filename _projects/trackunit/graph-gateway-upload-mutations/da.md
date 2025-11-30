---
title: "Trackunit: UploadMutations - graph-gateway"
categories: [trackunit, microservices]
tags: [graphql, rest, csharp, final]
lang: da
locale: da
nav_order: 43
ref: graph-gateway-upload-flow
---

Dette eksempel viser, hvordan **graph-gateway** håndterer upload-flowet via GraphQL-mutationer. `startUpload` og `confirmUpload` kalder interne REST-endpoints i **tu-ingestion-service** gennem en navngiven HttpClient (`ingestion`).  
`startUpload` returnerer den presigned upload-URL sammen med et `correlationId`, mens `confirmUpload` afslutter uploadprocessen ved at sende metadata om filen til **tu-ingestion-service**.

Begge mutationer kræver API-scope `RequireApiScope`.

```csharp
[ExtendObjectType("Mutation")]
public class UploadMutations
{
    [Authorize(Policy = "RequireApiScope")]
    public async Task<StartUploadPayload> StartUploadAsync(string filename, string contentType,
        [Service] IHttpClientFactory httpClientFactory, CancellationToken ct)
    {
        var http = httpClientFactory.CreateClient("ingestion");
        var request = new { filename, contentType };

        var response = await http.PostAsJsonAsync("/v1/uploads/start", request, ct);

        response.EnsureSuccessStatusCode();

        var imagePayload = await response.Content.ReadFromJsonAsync<ImageUploadPayload>(cancellationToken: ct)
               ?? throw new Exception("Empty response");

        string correlationId = Guid.NewGuid().ToString("n");
               
        return new StartUploadPayload(correlationId, imagePayload);
    }

    [Authorize(Policy = "RequireApiScope")]
    public async Task<ConfirmUploadPayload> ConfirmUploadAsync(string uploadId, int bytes, string checksum,
        [Service] IHttpClientFactory httpClientFactory, CancellationToken ct)
    {
        var http = httpClientFactory.CreateClient("ingestion");

        var request = new { uploadId, bytes, checksum };

        var response = await http.PostAsJsonAsync("/v1/uploads/confirm", request, ct);

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadFromJsonAsync<ConfirmUploadPayload>(cancellationToken: ct)
               ?? throw new Exception("Empty response");
    }
}
```