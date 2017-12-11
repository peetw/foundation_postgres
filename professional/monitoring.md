# Monitoring

## Database monitoring

Database monitoring consists of capturing and recording database events. This info
helps a DBA to detect, identify and fix potential database performance issues.
Monitoring statistics make it easier for a DBA to monitor the health
and performance of databases. Several monitoring tools are available.

## Database statistics

Database statistics catalog tables store database activity information. This
information is captured by the **Stats Collector** process, and includes:

* Current running sessions
* Running SQL
* Locks
* DML counts
* Row counts
* Index usage

The **Stats Collector** process collects and reports the information, and
adds a small overhead to query execution. This process can be disabled
by changing the ``track_counts`` and ``track_activities`` parameters.
The Stats Collector uses temp files stored in subdir ``pg_stat_tmp``.
Permanent stats are stored in the ``pg_catalog`` schema in the global
subdirectory.

**Performance tip**: point the ``stats_temp_directory`` at a RAM-based
file system (e.g. ramfs in Linux).

## DB statistics tables

* ``pg_catalog`` schema contains a set of tables, views and functions
  which store and report db stats
* ``pg_class`` and ``pg_stats`` catalog tables store all statistics
* ``pg_stat_database`` view can be used to view stats information about
  a database
* ``pg_stat_bgwriter`` shows background writer stats
* ``pg_stat_user_tables`` shows info about activities on a table like
  inserts, update, deletes, vacuums, etc.
* ``pg_stat_user_indexes`` shows info about index usage for all user tables

## Operating System processes

On Unix/Linux:

* ``ps``: info about current processes (eg. ``ps aux | grep postgres`` or ``ps -ef | grep postgres``)
* ``netstat``: info about current network connections (eg. ``netstat -an
  | grep LISTEN``)
* ``top`` or ``htop``: process list and system load

## Current sessions and locks

* ``pg_locks``: system table that stores info about outstanding locks in
  the lock manager
* ``pg_stat_activity``: shows the process ID, database, user, query and
  start time of current query (eg. ``SELECT * FROM pg_stat_activity WHERE pid = ...;``)
* Terminate a session gracefully using the ``pg_terminate_backend`` function
* Rollback a transaction gracefully using function ``pg_cancel_backend``

Don't terminate a process using OS command ``kill``, as this can leave
the database in a corrupted state.

## Logging slow-running queries

``log_min_duration_statement`` parameter sets a minimum statement
execution time (ms) that causes a statement to be logged. This can be
used to log all SQL that take longer than e.g. 10s to be logged. A
value of 0 will log all queries and their duration, while a value of
-1 will disable logging.

## Disk usage

Use ``pg_database_size(name)`` and ``pg_tablespace_size(name)`` to
display how much space is being used by a database or tablespace. Use
``pg_size_pretty`` to get the size in human readable format.

Example:

```sql
SELECT datname, pg_size_pretty(pg_database_size(datname)) FROM pg_database;
```

## Tracking execution statistics

Track query execution stats using the **pg_stat_statements** extension.
The extension tracks stats across all databases of a cluster. The view
``pg_stat_statements`` shows the query statistics. Use
``pg_stat_statements_reset`` function to reset the statistics.

Set up:

1. Add ``pg_stat_statements`` to ``shared_preload_libraries`` parameter
  in _postgresql.conf_
2. In _postgresql.conf_ configure: ``pg_stat_statements.max`` (default 5000),
  ``pg_stat_statements.track`` (top, all or none),
  ``pg_stat_statements.track_utility`` (default on), ``pg_stat_statements.save``
   (default on)
3. Restart the database cluster
4. Connect to a database and enable the ``pg_stat_statements`` extension
