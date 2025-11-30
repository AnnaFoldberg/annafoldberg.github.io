---
title: "Trackunit: RabbitMQConsumer - messaging NuGet-pakke"
categories: [trackunit, microservices]
tags: [rabbitmq, messaging, csharp, final]
lang: da
locale: da
nav_order: 41
ref: rabbitmqconsumer
---

**RabbitMQConsumer** i messaging NuGet-pakken opretter en connection og en channel, deklarerer en `fanout`-exchange og binder en queue dertil. `RunAsync` starter en asynkron consume-loop, hvor indkomne beskeder behandles via en handler-funktion. Handlerens resultat styrer, om beskeden ack’es eller nack’es uden requeue. Klassen understøtter automatisk genoprettelse af connections med `AutomaticRecoveryEnabled = true` og implementerer `DisposeAsync` for korrekt asynkron disposal af channel og connection.

```csharp
namespace Messaging.RabbitMQ;

public sealed class RabbitMQConsumer : IEventConsumer
{
    private readonly ConnectionFactory _factory;
    private IConnection? _connection;
    private IChannel? _channel;
    private string? _queue;

    public RabbitMQConsumer(string host, string user, string pass)
    {
        _factory = new ConnectionFactory
        {
            HostName = host,
            UserName = user,
            Password = pass,
            AutomaticRecoveryEnabled = true
        };
    }

    public async Task SubscribeAsync(string queue, string exchange, CancellationToken ct)
    {
        _connection = await _factory.CreateConnectionAsync(ct);
        _channel = await _connection.CreateChannelAsync();

        // Declare the fanout exchange
        await _channel.ExchangeDeclareAsync(exchange, ExchangeType.Fanout, durable: true, autoDelete: false);

        // Declare and bind the queue
        await _channel.QueueDeclareAsync(queue, durable: true, exclusive: false, autoDelete: false);
        await _channel.QueueBindAsync(queue, exchange, "");

        _queue = queue;
    }

    public async Task RunAsync(Func<ReadOnlyMemory<byte>, CancellationToken, Task<bool>> handler, CancellationToken ct)
    {
        var consumer = new AsyncEventingBasicConsumer(_channel!);
        consumer.ReceivedAsync += async (_, ea) =>
        {
            var ok = await handler(ea.Body, ct);
            if (ok) await _channel!.BasicAckAsync(ea.DeliveryTag, multiple: false, ct);
            else await _channel!.BasicNackAsync(ea.DeliveryTag, multiple: false, requeue: false, ct);
        };

        // Use manual ack since handler returns true or false
        await _channel!.BasicConsumeAsync(_queue!, autoAck: false, consumer, ct);
        await Task.Delay(Timeout.Infinite, ct);
    }

    public async ValueTask DisposeAsync()
    {
        if (_channel != null) await _channel.DisposeAsync();
        if (_connection != null) await _connection.DisposeAsync();
    }
}
```

**RabbitMQConsumer** implementerer **IEventConsumer**:

```csharp
namespace Messaging.RabbitMQ;

/// <summary>
/// Defines a consumer for events that subscribes to an exchange
/// and processes domain events published by other services.
/// </summary>
public interface IEventConsumer : IAsyncDisposable
{
    /// <summary>
    /// Subscribes to a RabbitMQ exchange by declaring the exchange and queue,
    /// and binding them together.
    /// </summary>
    /// <typeparam name="queue">Name of the queue that will receive events.</typeparam>
    /// <param name="exchange">Name of the exchange to subscribe to.</param>
    /// <param name="ct">Cancellation token.</param>
    /// <returns>A task that represents the asynchronous subscription setup operation.</returns>
    Task SubscribeAsync(string queue, string exchange, CancellationToken ct);

    /// <summary>
    /// Starts consuming events from the subscribed queue and invokes the specified handler
    /// for each received event payload.
    /// </summary>
    /// <typeparam name="handler">Delegate that processes each message payload.</typeparam>
    /// <param name="ct">Cancellation token.</param>
    /// <returns>A task that represents the lifetime of the consume loop.</returns>
    Task RunAsync(Func<ReadOnlyMemory<byte>, CancellationToken, Task<bool>> handler, CancellationToken ct);
}
```