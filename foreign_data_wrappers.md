# Foreign Data Wrappers

FDW enable access to external database or non-Postgres databases. Acts like an
adapter or interface to different data sources. Readonly FDWs introduced in 9.1,
read/write introduced in 9.3. Can also read non-relational data (CSV, JSON, XML).
All data will appear as a local PG table.

A FDW consists of two components:

1. Foreign table: an access method to relational/non-rel data
2. Data link: stores link/reference to other data source and provides
   transaction semantics, file protection and read/write access to data

FDWs are available as extensions. List: http://pgxn.org/tag/foreign%20data%20wrapper/

``file_fdw`` and ``postgres_fdw`` are available by default.

``CREATE FOREIGN DATA WRAPPER`` command can be used to write a custom FDW.

Set-up steps:

1. Enable extension
2. Create a foreign server
3. Create a user mapping
4. Create a foreign table
5. Query the foreign table

## Enable the FDW extension

``CREATE EXTENSION postgres_fdw;``

## Create a foreign server

Foreign server acts like an instance of an external data source,
encapsulates connection info needed by FDW to connect.

```sql
CREATE SERVER server_name FOREIGN DATA WRAPPER fdw_name OPTIONS (host
'hostname', port 'port', dbname 'my_db');
```

For example:

```sql
CREATE SERVER myserver FOREIGN DATA WRAPPER postgres_fdw OPTIONS (host
'foo', port '5432', dbname 'foodb');
```

## Create a user mapping (optional)

```sql
CREATE USER MAPPING FOR user SERVER server_name OPTIONS (user
'external_user', password 'password');
```

For example:

```sql
CREATE USER MAPPING FOR bob SERVER myserver OPTIONS (user 'bob', password 'secret');
```

## Create a foreign table

Link to the external data source using ``CREATE FOREIGN TABLE``
(local table that must match the remote data structure).

Can also use ``IMPORT FOREIGN SCHEMA`` to generate from external Postgres table(s).

## Query foreign table

Foreign tables can be queried as local tables. Note that **postgres_fdw**
supports SELECT, INSERT, UPDATE and DELETE but not all operations are
supported by all FDW extensions.

# Access control

Privileges can be assigned to foreign tables like local tables. User
mapping is used for user/password to remote server. Mapped remote used
must have privileges to access remote data.

```sql
GRANT USAGE ON FOREIGN DATA WRAPPER
GRANT USAGE ON FOREIGN TABLE
```
