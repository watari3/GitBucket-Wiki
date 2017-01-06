Since 4.0, GitBucket supports MySQL (>= 5.7) and PostgreSQL, not only embedded H2 database.

## Configuration

You can configure database connection in `GITBUCKET_HOME/database.conf`:

### H2 (default)

```
db {
  url = "jdbc:h2:${DatabaseHome};MVCC=true"
  user = "sa"
  password = "sa"
}
```

### MySQL 5.7

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
  url = "jdbc:postgresql://localhost/gitbucket"
  user = "test"
  password = "test"
}
```

## Data migration

Since 4.0, GitBucket also provide data exporting and importing.

![Data export and import](database_export.png)

If you have existing data in embedded H2 database, you can move your data to external database from H2 database by following operation:

1. At first, you must upgrade to GitBucket 3.14 (This is the final version of 3.x) and then, upgrade to GitBucket 4.x.
2. Export data as a SQL file from H2 database at the administration console
  - Exclude tables which is created by plug-ins if these plug-ins does not provide for GitBucket 4.x series. Recommend separate export for plug-in's tables. When 4.x supported version will be released, you can restore their data from the exported files.
3. Setup the external database and update `GITBUCKET_HOME/database.conf` as mentioned above and reboot GitBucket
4. Import an exported SQL file into the configured external database at the administration console

If you fail to import a SQL file, try to import it into your database directory. For example, you can import an exported SQL file using `mysql` command in MySQL as:

```
$ mysql -u root -p gitbucket < gitbucket-export-xxxxxxxx.sql
```

In the case of **PostgreSQL**, you might have to run following to the setup next value of sequences:

```sql
SELECT setval('label_label_id_seq', (select max(label_id) + 1 from label));
SELECT setval('activity_activity_id_seq', (select max(activity_id) + 1 from activity));
SELECT setval('access_token_access_token_id_seq', (select max(access_token_id) + 1 from access_token));
SELECT setval('commit_comment_comment_id_seq', (select max(comment_id) + 1 from commit_comment));
SELECT setval('commit_status_commit_status_id_seq', (select max(commit_status_id) + 1 from commit_status));
SELECT setval('milestone_milestone_id_seq', (select max(milestone_id) + 1 from milestone));
SELECT setval('issue_comment_comment_id_seq', (select max(comment_id) + 1 from issue_comment));
SELECT setval('ssh_key_ssh_key_id_seq', (select max(ssh_key_id) + 1 from ssh_key));
```
This operation has a risk to break your data by unexpected reason, so we strongly recommend to backup all your data in `GITBUCKET_HOME` before upgrading GitBucket.

## Migration of plugin data

For some plugins which use the database such as [gitbucket-gist-plugin](https://github.com/gitbucket/gitbucket-gist-plugin), you have to migrate plugin data also.

1. Uninstall these plugins temporarily before upgrading to GitBucket 4.x.
2. After upgrading to GitBucket 4.x, export GitBucket tables with plugin tables. (e.g. gitbucket-gist-plugin uses `GIST` and `GIST_COMMENT` table)
   - If you would like to upgrade plugins individually, you can export GitBucket data without plugin tables and export only plugin tables to other files. You can import plugin tables after upgrading your plugins.
3. Configure GitBucket to use external database, and install GitBucket 4.x supported version of plugins.
4. Run GitBucket, and import the data which is exported in 2.

## Database Encryption

Some databases support the encyrption of the DB specific files on disk. In order to activate such a functionality, the ```url``` specified in the ```GITBUCKET_HOME/database.conf``` file needs to adjusted according to the database encyrption specification.

For **H2 (default DB)**, the encryption steps are described here: http://www.h2database.com/html/features.html#file_encryption .

**Note:** If encryption is active, the DB Console https://github.com/gitbucket/gitbucket/wiki/Connect-to-H2-database and other plug-ins that use their own connectin also need those encryption adjustments *(as they don't rely on the standard ```GITBUCKET_HOME/database.conf``` to get their connection from)*.