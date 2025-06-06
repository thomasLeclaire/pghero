# Query Stats

The [pg_stat_statements module](https://www.postgresql.org/docs/current/static/pgstatstatements.html) is used for query stats.

## Installation

If you have trouble enabling query stats from the dashboard, try doing it manually.

Add the following to your `postgresql.conf`:

```conf
shared_preload_libraries = 'pg_stat_statements'
pg_stat_statements.track = all
pg_stat_statements.max = 10000
track_activity_query_size = 2048
```

Then restart PostgreSQL. As a superuser from the `psql` console, run:

```psql
CREATE extension pg_stat_statements;
```

#### Amazon RDS

Change `shared_preload_libraries` to `pg_stat_statements` in your [Parameter Group](https://console.aws.amazon.com/rds/home?region=us-east-1#parameter-groups:) and restart the database instance.

As a superuser from the `psql` console, run:

```psql
CREATE extension pg_stat_statements;
```

## Common Issues

#### pg_stat_statements must be loaded via shared_preload_libraries

Follow the instructions above.

#### The database user does not have permission to ...

The database user is not a superuser.  You can manually enable stats from the `psql` console with:

```psql
CREATE extension pg_stat_statements;
```

and reset stats with:

```psql
SELECT pg_stat_statements_reset();
```

#### Queries show up as `<insufficient privilege>`

For security reasons, only superusers can see queries executed by other users.

#### ERROR: column "total_plan_time" / "queryid" does not exist

Update the `pg_stat_statements` extension with:

```sql
ALTER EXTENSION pg_stat_statements UPDATE;
```
