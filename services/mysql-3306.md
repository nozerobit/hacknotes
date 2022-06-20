# MySQL - 3306

## MySQL Authentication

Connect to the database on localhost:

```text
mysql -u root -p -h 127.0.0.1
```

Connect to a remote database:

```text
mysql -u root -p -h 10.10.10.10
```

## MySQL Dump

We can dump a database with the following command:

```text
mysqldump --user=theseus --password=iamkingtheseus --host=localhost DB_NAME
mysqldump --user=theseus --password=iamkingtheseus --host=localhost Magic
```

Alternatively, we can dump all databases with:

```text
mysqldump -u theseus -p iamkingtheseus --all-databases
```

