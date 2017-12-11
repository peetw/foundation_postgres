# Extensions

Modules in the 'extension' directory are additional features to
Postgres. Generally they aren't included in the core database; they may
still be under development, tied to a PG version or not popular. Once
loaded they are available like builtin features. Extension modules are
located in ``/usr/share/postgresql/9.x/extension`` (Debian/Ubuntu).

Extensions are installed per-database using:

```sql
CREATE EXTENSION module_name;
```

To make extensions available to all future database, install an
extension in the **template1** database.

Extensions can only be installed by superusers.

## Extension module index

Several included Postgres extensions:

* Adminpack: file and log manipulation, used by pgAdmin
* Chkpass: auto-encrypted password datatype
* Cube: 
* dblink: remote query execution
* Eartdistance: compute Earth distance between two points
* Fuzzystrmatch: fuzzy string matching
* Hstore: module for store (key, value) pairs
* Isn: datatype for ISBN, ISSN, etc. product numbers
* Lo: large object maintenance
* Ltree: adds ltree data type for hierarchical treelike structure
* Pgcrypto: cryptographic functions
* pg_trgm: determine similarity of text strings
