---
title: "RabbitMQ: Exchange-typer"
categories: [microservices]
tags: [rabbitmq, communication]
lang: da
locale: da
nav_order: 8
ref: note-exchange-types
---
- **Direct Exchange:** Ruter beskeder til alle køer, der er tilknyttet denne exchange med samme routing key som den, udgiveren brugte.  
![Direct Exchange](../../../assets/images/notes/rabbitmq/exchange-types/direct-exchange.png)

- **Fanout Exchange:** Ruter beskeder til alle tilknyttede køer, og routing key ignoreres.  
![Fanout Exchange](../../../assets/images/notes/rabbitmq/exchange-types/fanout-exchange.png)

- **Topic Exchange:** Ruter beskeder baseret på mønstre i routing key’en. Hvis en binding er erklæret med et `#`, matcher dette symbol nul eller flere ord i routing key’en. Du kan også bruge `*`, som matcher et enkelt ord.  
![Topic Exchange](../../../assets/images/notes/rabbitmq/exchange-types/topic-exchange.png)

- **Headers Exchange:** Ignorerer routing key og kigger i stedet på headers, der er sendt med beskeden. Når en kø bindes til denne exchange, skal vi beslutte, om vi vil matche alle eller blot nogle af disse headers. Headers behøver ikke være strenge, de kan også være heltal eller dictionaries.  
![Headers Exchange](../../../assets/images/notes/rabbitmq/exchange-types/headers-exchange.png)

##### Use cases
- **Simple scenarier:** Direct  
    - Når du har et simpelt scenarie, der ikke involverer for mange exchanges, bindings og køer.  
- **Broadcasting:** Fanout  
    - Når din besked skal sendes til alle forbrugere.  
- **Komplekse scenarier:** Topic  
    - Når du forventer, at dit system skal skalere i fremtiden, eller du har brug for kategorisering eller filtrering. Topic exchanges giver den største fleksibilitet, hvis det kræves.  
- **Speciel filtrering:** Headers  
    - Anbefales kun, hvis du virkelig har brug for den særlige filtreringsmekanisme, den giver.  

<small>Kilde: [LinkedIn Learning: Learning RabbitMQ - The exchange types](https://www.linkedin.com/learning/learning-rabbitmq/the-exchange-types?resume=false&u=57075649)</small>  
<small>Kilde: [LinkedIn Learning: Learning RabbitMQ - Exchange type use cases](https://www.linkedin.com/learning/learning-rabbitmq/exchange-type-use-cases?resume=false&u=57075649)</small>