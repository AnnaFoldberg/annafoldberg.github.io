---
title: "Trackunit: RabbitMQConsumer - pacchetto NuGet messaging"
categories: [trackunit, microservices]
tags: [rabbitmq, messaging, csharp, final]
lang: it
locale: it
nav_order: 41
ref: rabbitmqconsumer
---

Il **RabbitMQConsumer** nel pacchetto NuGet di messaging crea una connection e un channel, dichiara un exchange di tipo `fanout` e vi associa una queue. `RunAsync` avvia un loop asincrono di consumo in cui i messaggi in arrivo vengono elaborati tramite una funzione handler. Il risultato dell’handler determina se il messaggio viene ackato oppure nackato senza requeue. La classe supporta il ripristino automatico della connessione tramite `AutomaticRecoveryEnabled = true` e implementa `DisposeAsync` per il corretto rilascio asincrono di channel e connection.

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

**RabbitMQConsumer** implementa **IEventConsumer**:

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