---
title: "Trackunit: UploadMutations - graph-gateway"
categories: [trackunit, microservices]
tags: [graphql, rest, csharp, final]
lang: en
locale: en
nav_order: 43
ref: graph-gateway-upload-flow
---

This example shows how **graph-gateway** handles the upload flow through GraphQL mutations. `startUpload` and `confirmUpload` call internal REST endpoints in **tu-ingestion-service** using a named HttpClient (`ingestion`).  
`startUpload` returns the presigned upload URL together with a `correlationId`, while `confirmUpload` completes the upload process by sending file metadata to **tu-ingestion-service**.

Both mutations require the API scope `RequireApiScope`.

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