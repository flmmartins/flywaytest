Tutorial link: https://flywaydb.org/getstarted/firststeps/commandline

### Location of Migration Files
Indicate locations is important otherwise he won't find migrations: `sqlMigrationPrefix`


### Prefix for Migration Files
Prefix for migration files start with `V`. If want to change it needs to be indicated


### Config file

Config file does not read conf/flyway.conf as indicates de tutorial, infact conf needs to be in the root
or explicit:

```
flyway -X -configFile=conf/flyway.conf migrate

```

Changing the name for flyway.properties didn't work inside a conf folder as described in `help`

**Solution:** Use flyway.conf inside project root

### Migrate

```
flyway -X -url=jdbc:sqlite:file:./foobardb -locations=filesystem:./sql/ migrate
```

### When migrate fauls

When migrate fail it will do rollback

Flyway 4.0.3 by Boxfuse


Database: jdbc:sqlite:file:./foobardb (SQLite 3.0)

Successfully validated 3 migrations (execution time 00:00.010s)

SQLite does not support setting the schema. Default schema NOT changed to main

Current version of schema "main": 2

Migrating schema "main" to version 3 - Delete people

ERROR: Migration of schema "main" to version 3 - Delete people failed! Changes successfully rolled back.

SQLite does not support setting the schema. Default schema NOT changed to main

ERROR: Migration V3__Delete_people.sql failed

SQL State  : null

Error Code : 1

Message    : [SQLITE_ERROR] SQL error or missing database (near "(": syntax error)

Location   : ./sql/V3__Delete_people.sql (/Users/fmartins/dev/LATAM/flyway/./sql/V3__Delete_people.sql)

Line       : 1

Statement  : delete from PERSON (ID, NAME) values (1, 'Axel')")")

The `flyway info` will display the failed migration as Pending:

Flyway 4.0.3 by Boxfuse

Database: jdbc:sqlite:file:./foobardb (SQLite 3.0)

SQLite does not support setting the schema. Default schema NOT changed to main

+---------+---------------------+---------------------+---------+
| Version | Description         | Installed on        | State   |
+---------+---------------------+---------------------+---------+
| 1       | Create person table | 2016-11-10 17:56:07 | Success |
| 2       | Add people          | 2016-11-10 17:57:31 | Success |
| 3       | Delete people       |                     | Pending |
+---------+---------------------+---------------------+---------+

### Clean

Clean will clean metadata table and your data

### Baseline

Apllying the wrong baseline will display:

```
flyway baseline -baselineVersion=3
```

Flyway 4.0.3 by Boxfuse

Database: jdbc:sqlite:file:./foobardb (SQLite 3.0)

SQLite does not support setting the schema. Default schema NOT changed to main

ERROR: Unable to baseline metadata table "main"."schema_version" with (3,<< Flyway 
Baseline >>) as it has already been initialized with (1,<< Flyway Baseline >>)

After baseline version 1, in `flyway info` will be like:

Flyway 4.0.3 by Boxfuse

Database: jdbc:sqlite:file:./foobardb (SQLite 3.0)
SQLite does not support setting the schema. Default schema NOT changed to main

+---------+-----------------------+---------------------+---------+
| Version | Description           | Installed on        | State   |
+---------+-----------------------+---------------------+---------+
| 1       | << Flyway Baseline >> | 2016-11-16 14:36:17 | Success |
| 2       | Add people            |                     | Pending |
| 3       | Delete people         |                     | Pending |
+---------+-----------------------+---------------------+---------+

After `flyway migrate` the migrations will appear with success