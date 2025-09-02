---
title: "RabbitMQ: Quickstart Guide"
categories: [microservices]
tags: [rabbitmq, guide]
lang: en
locale: en
nav_order: 9
ref: note-rabbitmq-quickstart-guide
---
##### Starting the Server
**Starting a Node in the Foreground**  
Runs RabbitMQ in the current terminal session. If you close the terminal, the RabbitMQ node shuts down. Useful for development, debugging, watching logs interactively, and quick testing.  

```sh
CONF_ENV_FILE="/opt/homebrew/etc/rabbitmq/rabbitmq-env.conf" /opt/homebrew/opt/rabbitmq/sbin/rabbitmq-server

# Enable feature flags (highly recommended):
/opt/homebrew/sbin/rabbitmqctl enable_feature_flag all
```

**Starting a Node in the Background**  
Runs RabbitMQ in the background with a service manager watching it. The process keeps running even if you close the terminal.  

```sh
brew services start rabbitmq

# Enable feature flags (highly recommended):
/opt/homebrew/sbin/rabbitmqctl enable_feature_flag all
```

##### Stopping the Server
```sh
brew services stop rabbitmq
```

##### Access RabbitMQ Management Console
http://localhost:15672/

##### User Management
**Default Login on Localhost**  
username: guest  
password: guest  

**Create new user**  
```sh
rabbitmqctl add_user myuser mypassword
```

**Make user admin**  
```sh
rabbitmqctl set_user_tags myuser administrator
```

**Give permissions**  
```sh
rabbitmqctl set_permissions -p / myuser ".*" ".*" ".*"
```
The three regexes are for:  
- **configure:** Create exchanges or queues  
- **write:** Publish messages  
- **read:** Consume messages  
".*" means “allow everything.” For tighter security, you can restrict these.