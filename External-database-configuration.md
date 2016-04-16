Since 4.0, GitBucket supports MySQL and PostgreSQL, not only embedded H2 database.

This feature is still **experimental** in GitBuckt 4.0, so try to use as your own risk.

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

If you have existing data in embedded H2 database, you can move your data to external database from H2 database by following operation:

1. At first, you must upgrade to GitBucket 3.14 (This is the final version of 3.x series)
2. Then upgrade to GitBucket 4.0
3. Export data from H2 database at the administration console
4. Configure external database and reboot GitBucket
5. Import exported data into the configured external database at the administration console

This operation has a risk to break your data by unexpected reason, so we strongly recommend to backup all your data in `GITBUCKET_HOME` before upgrading GitBucket.
