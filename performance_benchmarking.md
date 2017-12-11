# Performance benchmarking

Benchmarking measures the performance of a database and helps DBA in
preparing a system. It can evaluate changes and identify problem areas.

``pgbench`` is installed with Postgres. It connects multiple DB
sessions and runs the same SQL multiple times, then calculates the
average transaction rate. Workload involves five SELECT, UPDATE and
INSERT commands per transaction.

Invoke ``pgbench -i <database>`` to create the required tables in a database.
When creating the tables, can also specify the following options:

* -s: Scale factor to increase number of rows
* -F: Fill factor (default 100)

Options when running ``pgbench``:

* -c: # clients (default 1)
* -C: Establish a new connection for each transaction
* -t: # of transactions that each client runs (default 10)
* -T: Run the test for this many seconds
* -j: # of threads (default 1)

Example: ``pgbench -p 5444 -c 10 -T 30 testdb``

Other popular tools include BenchmarkSQL or HammerDB.
