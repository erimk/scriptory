# Restoring a PostgreSQL Backup

## Requirements

- A backup file: either `FILENAME.backup` or `FILENAME.sql`  
- PostgreSQL client installed  
- For `.backup` files, the PostgreSQL client **must match** the version used to generate the backup (e.g., `psql --version 16`)

## Recreate the Database
Drop and recreate the target database:

```bash
PGPASSWORD=postgres psql -h localhost -U postgres -d postgres -c "DROP DATABASE IF EXISTS name_db;"
PGPASSWORD=postgres psql -h localhost -U postgres -d postgres -c "CREATE DATABASE name_db;"
```

## Install Required Extensions

Before restoring the database, make sure the required PostgreSQL extensions are installed:

```bash
PGPASSWORD=postgres psql -h localhost -U postgres -d name_db -c "CREATE EXTENSION IF NOT EXISTS citext;"
PGPASSWORD=postgres psql -h localhost -U postgres -d name_db -c "CREATE EXTENSION IF NOT EXISTS pg_trgm;"
```


##### Restore the Backup
 - If you have a .backup file:

`PGPASSWORD=postgres pg_restore --clean --if-exists --no-owner -h localhost -U postgres -d name_db <FILENAME>.backup`

 - If you have a .sql file:

`PGPASSWORD=postgres psql --set ON_ERROR_STOP=on -h localhost -U postgres name_db < <FILENAME>.sql`
