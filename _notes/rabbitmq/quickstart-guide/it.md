---
title: "Guida Rapida a RabbitMQ"
categories: [microservices]
tags: [rabbitmq, guide]
lang: it
locale: it
nav_order: 9
ref: note-rabbitmq-quickstart-guide
---
##### Avvio del server
**Avviare un nodo in primo piano**  
Esegue RabbitMQ nella sessione del terminale corrente. Se chiudi il terminale, il nodo RabbitMQ si arresta. Utile per sviluppo, debugging, visualizzazione interattiva dei log e test rapidi.  

```sh
CONF_ENV_FILE="/opt/homebrew/etc/rabbitmq/rabbitmq-env.conf" /opt/homebrew/opt/rabbitmq/sbin/rabbitmq-server

# Abilitare i feature flag (fortemente consigliato):
/opt/homebrew/sbin/rabbitmqctl enable_feature_flag all
```

**Avviare un nodo in background**
Esegue RabbitMQ in background con un service manager che lo monitora. Il processo continua a funzionare anche se chiudi il terminale.

```sh
brew services start rabbitmq

# Abilitare i feature flag (fortemente consigliato):
/opt/homebrew/sbin/rabbitmqctl enable_feature_flag all
```

##### Arrestare il server
```sh
brew services stop rabbitmq
```

**Accedere alla Management Console di RabbitMQ**  
http://localhost:15672/

##### Gestione utenti
**Login predefinito su localhost**  
username: guest  
password: guest  

**Creare un nuovo utente**
```sh
rabbitmqctl add_user myuser mypassword
```

**Rendere l’utente amministratore**  
```sh
rabbitmqctl set_user_tags myuser administrator
```

**Concedere permessi**
```sh
rabbitmqctl set_permissions -p / myuser ".*" ".*" ".*"
```
Le tre regex sono per:  
- **configure:** Creare exchange o code
- write:** Pubblicare messaggi
- read:** Consumare messaggi
“.*” significa “consenti tutto.” Per maggiore sicurezza, puoi restringere questi permessi.