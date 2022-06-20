# PostgreSQL - 5432,5433

## PostgreSQL Version Enumeration

We can connect remotely to PSQL:

```
psql -h <IP> -U postgres -p <PORT>
```

You may notice a banner which it reveals the version. Alternatively, you can just use this SQL statement to check the current version:

```
SELECT version();
```

You can also instruct PostgreSQL to show the value associated with the _server\_version_ parameter:

```
SHOW server_version;
```

PSQL Version:&#x20;

{% embed url="https://phoenixnap.com/kb/check-postgresql-version" %}
