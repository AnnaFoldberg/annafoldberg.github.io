---
title: "RabbitMQ Hurtigstart-guide"
categories: [microservices]
tags: [rabbitmq, guide]
lang: da
locale: da
nav_order: 9
ref: note-rabbitmq-quickstart-guide
---
##### Start serveren
**Start en node i forgrunden**  
Kører RabbitMQ i den nuværende terminalsession. Hvis du lukker terminalen, stopper RabbitMQ-noden. Nyttig til udvikling, debugging, at se logs interaktivt og hurtig test.  

```sh
CONF_ENV_FILE="/opt/homebrew/etc/rabbitmq/rabbitmq-env.conf" /opt/homebrew/opt/rabbitmq/sbin/rabbitmq-server

# Aktivér feature flags (stærkt anbefalet):
/opt/homebrew/sbin/rabbitmqctl enable_feature_flag all
```

**Start en node i baggrunden**  
Kører RabbitMQ i baggrunden med en service manager, der overvåger den. Processen fortsætter med at køre, selvom du lukker terminalen.
```sh
brew services start rabbitmq

# Aktivér feature flags (stærkt anbefalet):
/opt/homebrew/sbin/rabbitmqctl enable_feature_flag all
```

##### Stop serveren
```sh
brew services stop rabbitmq
```

**Tilgå RabbitMQ Management Console**  
http://localhost:15672/

##### Brugerstyring
**Default login på localhost**  
brugernavn: guest
adgangskode: guest

**Opret ny bruger**  
```sh
rabbitmqctl add_user myuser mypassword
```

**Gør bruger til admin**  
```sh
rabbitmqctl set_user_tags myuser administrator
```

**Giv rettigheder**  
```sh
rabbitmqctl set_permissions -p / myuser ".*" ".*" ".*"
```
De tre regexer er for:
- **configure:** Opret exchanges eller køer
- **write:** Publicér beskeder
- **read:** Konsumér beskeder
".*" betyder “tillad alt.” For strammere sikkerhed kan du begrænse disse.