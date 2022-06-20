# Modbus - 502

## Modbus Information

Modbus Protocol is a messaging structure created in 1979 by Modicon. It is used to connect intelligent devices via master-slave/client-server communication.

Default port: 502

```text
PORT    STATE SERVICE
502/tcp open  modbus
```

## Enumeration

```text
nmap --script modbus-discover -p 502 <IP>
```

