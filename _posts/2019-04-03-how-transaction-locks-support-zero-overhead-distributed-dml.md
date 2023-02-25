---
title: "How Transaction Locks support Zero Overhead Distributed DML"
date: "2019-04-03"
categories: 
  - "nuodb"
---

Let us imagine a scenario that needs to prevent MVCC write skews...

One transaction increases the salary of everyone in a department by 10%; another transaction inserts a new employee with a salary X. Since the two transactions do not conflict, MVCC does not prevent either of them from committing. After both resolve, the overall salary in the department could be above the budget. To prevent a similar situation, the application developer might want to have exclusive access to the table.

NuoDB 3.2.2 exposes a new type of lock that guarantees exclusive access to a resource across the distributed cluster. We call them Transactional Locks and they are the underlying mechanism powering the new NuoDB [LOCK statement](https://doc.nuodb.com/Latest/Content/LOCK.htm).

## ZERO NORMAL CASE OVERHEAD

NuoDB is a distributed database. When we were designing Transactional Locks, our primary concern was the happy path - the DML that you execute thousands of times a second, does not pay the cost of a distributed operation.

Read DML transactions (select) will never acquire any lock. The consistency is guaranteed by [Multi Version Concurrency Control](https://www.nuodb.com/techblog/mvcc-part-1-overview). A Consistent Read transaction containing multiple select statements keeps a consistent metadata model of the database regardless of concurrent schema modifications.

Write DML transactions (update, insert, delete, select for update) acquire SHARED locks. These locks are local to the Transaction Engine that is running the transaction. Unless there is an EXCLUSIVE lock already acquired, acquiring this lock will always succeed and does not conflict with any other read or write DML operation. These SHARED locks are not distributed, serialized, communicated, or replicated throughout the cluster. Truly zero overhead.

## PAYING THE COST: TRANSACTIONS THAT REQUIRE UNIQUE ACCESS

Nothing in life comes for free, and neither does distributed consistency. Since SHARED locks are not known throughout the cluster, any operation that needs EXCLUSIVE access to a resource needs to pay the cost.

A operation, which requires EXCLUSIVE access to the altered resource, needs to wait for all transactions on all Transaction Engines with SHARED locks to finish. Once all transactions have been resolved (committed or rolled back), the requestor has exclusive access.

Transactional Locks make sure that the requesting operation eventually acquires the EXCLUSIVE lock, avoiding any starvation. As a NuoDB application developer, you should make sure that there are no long running write DML statements in flight. The exclusive requestor will have to wait for any long running transactions, possibly locking out other writers. Long running analytical SELECT queries will not conflict with transactional locks.

## LOCK SQL STATEMENT

All write DML acquires SHARED locks automatically and without any user intervention. As of 3.2.2, no EXCLUSIVE locks are acquired automatically.

As described in the introduction of this article, there might be situations when your application wants to have unique access to a resource. We offer you the [LOCK statement](http://doc.nuodb.com/Latest/Content/LOCK.htm)that does exactly that. Just keep in mind that anything that is exclusive and/or unique in a distributed system can have impact on the whole cluster.

When using Transactional Locks, NuoDB recommends the following:

- Make sure you have [AUTOCOMMIT OFF](http://doc.nuodb.com/Latest/Content/About-Explicit-Transactions.htm) or that you started a new transaction via [START TRANSACTION](http://doc.nuodb.com/Latest/Content/START-TRANSACTION.htm).
- Your transaction uses [READ COMMITTED](http://doc.nuodb.com/Latest/Content/Description-of-NuoDB-Transaction-Isolation-Levels.htm). Consistent read transactions can not see all changes that happened after a snapshot was taken. This can result in lots of update conflicts on the locking transaction that will prevent you from changing the metadata.
- Do not have long running DML transactions.

We do not offer the ability to manually lock a resource in SHARED mode. All write DML acquires a SHARED lock. To prevent the acquisition of a EXCLUSIVE LOCK on a table, NuoDB recommends executing a dummy update _update t set i = i where 0 = 1;_

## THERE IS NO UNLOCK

Transactional locks cannot be released voluntarily. The only way to unlock a resource is to commit or roll back the transaction.

Once obtained, the lock is held for the remainder of the current transaction. There is no UNLOCK TABLE command; locks are always released at the transaction end.

## MORE READING

We will explore the technical fundamentals of transactional locks in [future articles](https://www.martinkysel.com/distributed-transactional-locks/).

This article first appeared at the [NuoDB Tech Blog](https://www.nuodb.com/techblog/quick-dive-nuodb-architecture) under the name [How Transaction Locks support Zero Overhead Distributed DML](https://www.nuodb.com/techblog/how-transaction-locks-support-zero-overhead-distributed-dml)
