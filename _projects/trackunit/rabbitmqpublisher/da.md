---
title: "Trackunit: RabbitMQPublisher - messaging NuGet-pakke"
categories: [trackunit, microservices]
tags: [rabbitmq, messaging, csharp, final]
lang: da
locale: da
nav_order: 39
ref: rabbitmqpublisher
---

**RabbitMQPublisher** i messaging-NuGet-pakken etablerer en delt, genbrugelig og trådsikker forbindelse til **RabbitMQ** og publicerer events. `PublishAsync` sikrer, at forbindelsen er initialiseret, opretter en channel, deklarerer exchangen idempotent og sender event-payloaden som JSON med persistente beskeder. Klassen understøtter automatisk genoprettelse af connections med `AutomaticRecoveryEnabled = true` og implementerer `DisposeAsync` for korrekt asynkron disposal af channel og connection.

```csharp
namespace Messaging.RabbitMQ;

/// <summary>
/// RabbitMQ publisher that ensures a single shared connection and
/// publishes events to configured exchanges.
/// Implements <see cref="IEventPublisher"/> so services can interact with RabbitMQ without transport details.
/// </summary>
public sealed class RabbitMQPublisher : IEventPublisher, IAsyncDisposable
{
    private readonly string _host, _user, _pass;
    private readonly int _port;
    private IConnection? _connection;
    private static readonly JsonSerializerOptions _jsonOpts = new(JsonSerializerDefaults.Web);

    // Ensures that only one caller at a time can initialize the connection
    private readonly SemaphoreSlim _initLock = new(1, 1);

    /// <summary>
    /// Creates a new RabbitMQ publisher with credentials and hostname.
    /// </summary>
    public RabbitMQPublisher(string host, string user, string pass)
    : this(host, user, pass, 5672) { }

    /// <summary>
    /// Creates a new RabbitMQ publisher with credentials and hostname and port.
    /// </summary>
    public RabbitMQPublisher(string host, string user, string pass, int port)
        => (_host, _user, _pass, _port) = (host, user, pass, port);

    /// <summary>
    /// Lazily initializes the RabbitMQ connection in a thread-safe manner.
    /// Ensures only one connection attempt happens at a time.
    /// </summary>
    private async Task EnsureConnectionAsync(CancellationToken ct)
    {
        // Already connected and open
        if (_connection is { IsOpen: true }) return;

        await _initLock.WaitAsync(ct).ConfigureAwait(false);
        try
        {
            if (_connection is { IsOpen: true }) return;

            var factory = new ConnectionFactory {
                HostName = _host,
                UserName = _user,
                Password = _pass,
                Port = _port,
                AutomaticRecoveryEnabled = true
            };

            // Establish new connection
            _connection = await factory.CreateConnectionAsync(ct).ConfigureAwait(false);
        }
        finally
        {
            _initLock.Release();
        }
    }

    public async Task PublishAsync<T>(string exchange, string routingKey, T @event,
        ExchangeKind kind = ExchangeKind.Fanout, CancellationToken ct = default)
    {
        await EnsureConnectionAsync(ct).ConfigureAwait(false);

        await using var channel = await _connection!.CreateChannelAsync().ConfigureAwait(false);

        // RabbitMQ's ExchangeType class is constants, so we can use the enum name lower-cased.
        var type = kind.ToString().ToLowerInvariant();

        await channel.ExchangeDeclareAsync(exchange, type: type, durable: true, autoDelete: false, arguments: null)
                .ConfigureAwait(false);

        var payload = Encoding.UTF8.GetBytes(JsonSerializer.Serialize(@event, _jsonOpts));

        var props = new BasicProperties { ContentType = "application/json", DeliveryMode = DeliveryModes.Persistent };

        await channel.BasicPublishAsync<BasicProperties>(exchange, routingKey, mandatory: false, basicProperties: props, body: payload, cancellationToken: ct)
                .ConfigureAwait(false);
    }

    /// <summary>
    /// Disposes the RabbitMQ connection and associated resources.
    /// </summary>
    public async ValueTask DisposeAsync()
    {
        if (_connection is not null)
            await _connection.DisposeAsync().ConfigureAwait(false);

        _initLock.Dispose();
    }
}
```

**RabbitMQPublisher** implementerer **IEventPublisher**:

```csharp
namespace Messaging.RabbitMQ;

/// <summary>
/// Defines a publisher for events that notifies other services
/// about something that has already happened.
/// </summary>
public interface IEventPublisher
{
    /// <summary>
    /// Publishes an event to a RabbitMQ exchange with the given routing key.
    /// If the exchange does not exist, it is declared first.
    /// </summary>
    /// <typeparam name="T">Type of the message payload.</typeparam>
    /// <param name="exchange">Name of the exchange.</param>
    /// <param name="routingKey">Routing key for the message.</param>
    /// <param name="@event">Message payload (serialized to JSON).</param>
    /// <param name="kind">Exchange type (direct, fanout, topic, header).</param>
    /// <param name="ct">Optional cancellation token.</param>
    /// <returns>A task that represents the asynchronous publish operation.</returns>
    Task PublishAsync<T>(string exchange, string routingKey, T @event, ExchangeKind kind = ExchangeKind.Fanout, CancellationToken ct = default);
}
```