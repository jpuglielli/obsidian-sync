

## Transactions
#### Isolation levels
1. Read Uncommitted? NOT SUPPORTED
2. **Read Committed**
	- Only reads data that has been committed.
	- Each **statement** within the transaction sees a fresh snapshot. So if another transaction commits between your two SELECT statements, you'll get different results.
3. **Repeatable Read**
	- Creates snapshot of the database at the start of the _first query_ in the transaction
	- Does not hold **read** locks, since PostgreSQL uses [[MVCC]], but can use other database resources
		- Writes hold row level locks
	- If writes are made on rows which change while this happens, fails with serialization error.
4. **Serializable**
	- All transactions behave as if they were executed serially. PostgreSQL achieves this using SSI (Serializable Snapshot Isolation), which lets transactions run concurrently on snapshots but tracks read/write dependencies between them. If it detects a dependency cycle that couldn't occur in any serial execution order, it aborts one transaction with a serialization error. Your code must be prepared to retry these.
#### `UNNEST` Function
- is specifically for array types in PostgreSQL
- That said, the _concept_ of expanding a collection into rows has analogs for other composite types:
	- 