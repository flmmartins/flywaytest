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


