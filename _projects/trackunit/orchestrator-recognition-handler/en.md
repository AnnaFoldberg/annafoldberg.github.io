---
title: "Trackunit: Handling Completed Image Analysis - svc-analysis-orchestrator"
categories: [trackunit, microservices]
tags: [rabbitmq, messaging, csharp, final]
lang: en
locale: en
nav_order: 42
ref: orchestrator-recognition-handler
---

This example shows how **svc-analysis-orchestrator** handles completed image analyses. It consumes `tu.recognition.completed` via the `orchestrator.recognition-completed` queue, deserializes the payload, and then publishes an `AnalysisCompleted` event to the `analysis.completed` fanout exchange.

```csharp
namespace AnalysisOrchestrator.Workers;

public sealed class RecognitionCompletedWorker : BackgroundService
{
    private readonly IEventPublisher _publisher;
    private readonly IEventConsumer _consumer;
    private static readonly JsonSerializerOptions JsonOpts = new(JsonSerializerDefaults.Web);

    public RecognitionCompletedWorker(IEventPublisher publisher, IEventConsumer consumer)
    {
        _publisher = publisher;
        _consumer = consumer;
    }

    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        try
        {
            // Declare queue
            const string queue = "orchestrator.recognition-completed";

            // Subscribe to RecognitionCompleted fanout exchange
            Console.WriteLine($"[Orchestrator] Subscribing to exchange '{Exchanges.RecognitionCompleted}'");
            await _consumer.SubscribeAsync(queue, Exchanges.RecognitionCompleted, ct);
            // Start consume loop
            await _consumer.RunAsync(HandleRecognitionCompletedAsync, ct);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"[Orchestrator] Error: {ex.Message}");
            await Task.Delay(TimeSpan.FromSeconds(3), ct);
        }
    }

    private async Task<bool> HandleRecognitionCompletedAsync(ReadOnlyMemory<byte> body, CancellationToken ct)
    {
        RecognitionCompleted? payload;
        try
        {
            payload = JsonSerializer.Deserialize<RecognitionCompleted>(body.Span, JsonOpts);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"[Orchestrator] Failed to deserialize RecognitionCompleted: {ex.Message}");
            return false; // Nack: payload is broken, don't requeue
        }
        if (payload == null)
        {
            Console.WriteLine("[Orchestrator] RecognitionCompleted payload was null");
            return false;
        }
        
        Console.WriteLine($"[Orchestrator] Received RecognitionCompleted");

        // Publish AnalysisCompleted
        var completed = new AnalysisCompleted(Success: true, payload);
        await _publisher.PublishAsync(Exchanges.AnalysisCompleted, "", completed, ExchangeKind.Fanout, ct);
        
        Console.WriteLine($"[Orchestrator] Published AnalysisCompleted");

        return true; // Ack
    }
}
```