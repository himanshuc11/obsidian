
### Transaction
A transaction is a single task with contain multiple operations taking place wholly or not at all. 

This is done at a query level automatically by the database on query level.
```plsql
update marks set math_marks = math_marks + 10 where total_marks > 50 and total_marks < 60;
```
Here all the rows which satisfy the condition are updated or none of the rows are updated.

To define a transaction that spans across queries (Transaction block)
```plsql
begin;
query1
query2
query3
.
.
.
queryn
commit;
```
Here all queries succeed or even if one fails, all of them fail.

Transactions start with begin, commit tells the db to write all the data to disk and mark the transaction as success. In case of failure Postgres automatically rolls back.

To manually reset a transaction we can use
1. rollback
2. set a save point and rollback to.

Note: For single queries DB implicitly wraps query in begin and commit if we don't do it ourself. 

**Possible implementation of transaction blocks**
1. When a query is finished write to the disk. q1 success write to disk, q2 success write to disk and so on. Here the final `commit` (execution of line commit) will be quick as everything is already written to disk. 
   
   Tradeoff is rollback is very costly as we need to re-write everything to disk and then mark the rollback as complete
   
2. When a query is finished keep the results in memory only. Hence the final `commit` execution will be slow as we need to write everything in memory on the disk. 
   
   Tradeoff is rollback is quick as we need to flush the memory, not the disk.

Some other factors to consider are that, what if db crashes mid commit or mid rollback (electricity goes off etc). DB needs a mechanism to make sure that db after coming back up, first makes data consistent (fully commit or fully rollback) and then accepts new queries.

Note: Transactions can be read only as we don't want our data to be updated while we process it and generate a view/report of it.

### Atomicity
All query in transaction must succeed, if one of the query fails (due to failed constraint duplicate primary key etc) the whole transaction fails and must roll back the transaction.

DB can optimise for 

1. Commit
   - Optimistic that mostly commits will be successful.
   - We write to disk for each successful query.
	   - If all queries successful, just mark the transaction as finished (`commit` execution line).
	   - If any query fails, need to rollback, which will be slow as fuck as we need to re-write on disk. 

	Fast commit, slow rollback 
2. Rollback
	 - Assume that mostly commits will be rolled back
	 - We write to memory (buffer) for each successful query
		 - If all queries successful, and we reach the `commit` line, we need to write all the buffer data to the disk
		 - If any query fails, just need to flush the memory
		
	Fast rollback, slow commit.

```plsql
begin;
q1
q2
...
qn
commit;
```
 If `q1`, `q2` ... , `qk` are committed (`commit` is being run) and db crashes, then according to atomicity principle it is the job of db to ensure that on restart db reaches a consistent state.
 Either by rolling back the whole transaction or committing the whole transaction.

Note: This is the reason longer transactions are bad, they have a rollback issue.

### Durability
Non volatile storages are storages that are safe from power loss, os failure and hardware failure.
Process of persisting **committed** writes to a non volatile storage.

**OS Cache**
To speed up writes (i/o operations), whenever an application requests OS to write to a disk, the OS caches the write and tries to batch multiple writes so that i/o becomes more performant.

To write to disk without caching (as db committed writes need to be in a non volatile storage), db use `fsync` os to flush to the disk. fsync is an expensive operation and slows down the whole system.

Techniques
1. WAL (Write Ahead Log)
   Writes to db are heavy(index, B+ tree update etc), so we maintain a log file with deltas changes (like git diff). It is the temporary stage between data in memory and data yet to be written on disk. 
   
   When user writes to database
	   - Log entry written to WAL
	   - Periodically sync the WAL changes to db.
	
	There are multiple WAL files, each file capped at 16Mb.
	
	Advantages of WAL
	1. No need to flush data to disk on each commit, which reduces number of writes
	2. Periodically sync data to the table structure.
	3. In case of db/os crash, recovery of db through logs.
	4. Point in time recovery (Snapshot recovery), can construct the exact state of database at a particular instance of time.

	Contents of WAL file
	1. LSN (Byte offset of the entry) {Need more research}
	2. CRC code to validate data integrity to make sure WAL is not corrupted
	3. Record of the actual diff

	WAL things to understand more.
	1. LSN of WAL file
	2. Look at an actual WAL file from Postgres
	3. An update took place, then we write data to WAL, but don't sync with disk yet. So when we do `select columnname from table`, how does this query return result that are in WAL but not in disk.

### Isolation
Inflight transaction are transaction that are currently running.
Degree of interaction between two inflight transactions

Isolation issues
- One transaction reads data, currently being modified by another transaction.
- Two transactions try to simultaneously modify the same data.
Note: Two transactions both reading is not an issue as data is never updated.

To fix these isolation issues we use isolation levels of transactions.
Serializable isolation(run next transaction only when current one is finished) is possible but has performance implications, hence we can opt into weaker levels of isolation.

