# Toolbox Configuration Templates

## Cloud SQL Postgres

**Required Information:**
- Google Cloud Project ID
- Region
- Instance Name
- Database Name
- Database User
- Database Password

**Template:**

```yaml
sources:
  postgres-db:
    kind: cloud-sql-postgres
    project: <project_id>
    region: <region>
    instance: <instance_name>
    database: <database_name>
    user: <user>
    password: <password>
tools:
  list_schemas:
    kind: postgres-list-tables
    source: postgres-db
    description: Use this tool to list all tables and their schemas in the postgres-db database.
```

## Cloud SQL MySQL

**Required Information:**
- Google Cloud Project ID
- Region
- Instance Name
- Database Name
- Database User
- Database Password

**Template:**

```yaml
sources:
  mysql-db:
    kind: cloud-sql-mysql
    project: <project_id>
    region: <region>
    instance: <instance_name>
    database: <database_name>
    user: <user>
    password: <password>
tools:
  list_schemas:
    kind: mysql-list-tables
    source: mysql-db
    description: Use this tool to list all tables and their schemas in the mysql-db database.
```

## AlloyDB Postgres

**Required Information:**
- Google Cloud Project ID
- Region
- Cluster ID
- Instance ID
- Database Name
- Database User
- Database Password

**Template:**

```yaml
sources:
  alloydb-db:
    kind: alloydb-postgres
    project: <project_id>
    region: <region>
    cluster: <cluster_id>
    instance: <instance_id>
    database: <database_name>
    user: <user>
    password: <password>
tools:
  list_schemas:
    kind: postgres-list-tables
    source: alloydb-db
    description: Use this tool to list all tables and their schemas in the alloydb-db database.
```

## Spanner

**Required Information:**
- Google Cloud Project ID
- Instance ID
- Database Name

**Template:**

```yaml
sources:
  spanner-db:
    kind: spanner
    project: <project_id>
    instance: <instance_id>
    database: <database_name>
tools:
  list_schemas:
    kind: spanner-list-tables
    source: spanner-db
    description: Use this tool to list all tables and their schemas in the spanner-db database.
```