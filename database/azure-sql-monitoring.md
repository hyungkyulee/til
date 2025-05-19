## 


## Work threads
In Azure SQL Database, “work threads” (or worker threads) refer to the underlying threads used by the SQL engine to execute queries and perform tasks such as data retrieval, inserts, updates, and index maintenance. These are part of the SQL Server engine’s internal thread pool that handles concurrent operations.

Component | Relates To | Description
Worker Thread | SQL Engine | Each active query uses a SQL worker thread. Limited on SQL Server based on resources.
Max Pool Size | ADO.NE T | Limits number of concurrent open connections from the app to SQL. Prevents overload.
EF DbContext | ADO.NET Pool | Each uses a connection from the pool temporarily.
EF Connection | Worker Thread | Only when an EF query is active and executing in SQL is a worker thread used.

1. Worker Threads (SQL Server / Azure SQL Engine)
	•	These are internal threads used by the SQL engine to execute commands (queries, inserts, updates, etc.).
	•	One worker thread per active SQL request (not per connection).
	•	SQL Server has a thread pool, and if it’s exhausted, new requests will wait (often seen as THREADPOOL waits).

2. Max Pool Size (ADO.NET Connection Pooling)
	•	Defined in the connection string (e.g., Max Pool Size=100).
	•	Controls the maximum number of physical SQL connections that can be reused in a pool.
	•	Prevents overload on SQL by limiting the number of concurrent connections from your app.
	•	If all connections in the pool are in use, new DbContext instances trying to open a connection will block (wait) or eventually throw a timeout.

Important: This is client-side (ADO.NET/.NET side), not in SQL itself.

⸻

3. EF DbContext
	•	EF’s main unit of work; wraps a SQL connection.
	•	One DbContext = one logical connection, which uses a pooled ADO.NET connection under the hood.
	•	When you .SaveChanges() or .ToList(), EF opens a connection, runs the query, then closes it (returning it to the pool if pooling is enabled).

⸻

4. EF Session / Connection
	•	EF Core uses DbConnection (from ADO.NET) behind the scenes.
	•	When a DbContext opens a connection, it pulls it from the ADO.NET connection pool (unless explicitly opened or kept open).
	•	You don’t deal with “sessions” in EF like you do in ORMs such as NHibernate, but the connection lifetime is controlled by the DbContext’s lifetime.





