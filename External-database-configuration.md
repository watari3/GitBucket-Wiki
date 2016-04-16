Since 4.0, GitBucket supports MySQL and PostgreSQL, not only embedded H2 database.

## Configuration

You can configure database connection in `GITBUCKET_HOME/database.conf`:

### MySQL

```
db {
  url = "jdbc:mysql://localhost/gitbucket?useUnicode=true&characterEncoding=utf8"
  user = "test"
  password = "test"
}
```

### PostgreSQL

```
db {
  url = "jdbc:postgresql://192.168.99.100/gitbucket"
  user = "test"
  password = "test"
}
```

## Data migration

Since 4.0, GitBucket also provide data exporting and importing.

![Data export and import](database_export.png)

If you have existing data in embedded H2 database, you can move your daba as following sequence:

1. At first, you must upgrade to GitBucket 3.14 (This is the final version of 3.x series)
2. Then upgrade to GitBucket 4.0
3. Export data from H2 database at the administration console
4. Configure external database and reboot GitBucket
5. Import exported data into the configured external database at the administration console

