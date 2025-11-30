---
title: "Trackunit: UploadMutations - graph-gateway"
categories: [trackunit, microservices]
tags: [graphql, rest, csharp, final]
lang: it
locale: it
nav_order: 43
ref: graph-gateway-upload-flow
---

Questo esempio mostra come **graph-gateway** gestisce il flusso di upload tramite mutation GraphQL. `startUpload` e `confirmUpload` chiamano endpoint REST interni in **tu-ingestion-service** tramite un HttpClient nominato (`ingestion`).  
`startUpload` restituisce l’URL di upload presigned insieme a un `correlationId`, mentre `confirmUpload` completa il processo di upload inviando i metadati del file a **tu-ingestion-service**.

Entrambe le mutation richiedono lo scope API `RequireApiScope`.

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