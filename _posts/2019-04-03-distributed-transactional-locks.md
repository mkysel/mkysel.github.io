---
title: "Distributed Transactional Locks"
date: "2019-04-03"
categories: 
  - "nuodb"
---

As I explained in a previous blog post, sometimes MVCC is not sufficient and an operation needs to block out all other concurrent modifications. NuoDB is able to lock three types of lockable resources: tables, schemas, and sequences. A resource can either be locked in SHARED mode (which still allows record modification, but no metadata modification) or EXCLUSIVE which prevents any concurrent modification.

EXCLUSIVE access is required for DDL that visits all records (various index operations for example) and distributed concurrent access is not possible. Exclusive access can also be used by operations that need to work around MVCC write skew anomalies.

SHARED locks need to be fast, consume as little memory as possible, involve no additional nodes in the cluster, and cause no additional messaging.

EXCLUSIVE locks, on the other hand, need to pay the cost.

MVCC uses row locks to serialise updates to a single record. Both transactional locks and row locks participate in the same deadlock detection process, but are otherwise independent. The following table explains the interaction between transactional locks and MVCC row locks. If you are interested in how row locks work, I recommend reading our [MVCC blog series](https://www.nuodb.com/techblog/mvcc-part-1-overview).

| Row Locks (DML) / Transactional Locks | Shared | Exclusive |
| --- | --- | --- |
| select | No Conflict | No Conflict |
| insert, update, delete, select for update | No Conflict | Conflict |

### USE OF TRANSACTIONAL LOCKS

Transactional locks are tied to the lifetime of a transaction. A lock can not exist without a transaction and it can not be released while the transaction still exists. The only way to release a transactional lock, is to resolve the transaction. There are currently three ways to resolve a transaction: commit, rollback, and failure (local or cluster wide).

The lock is held until all effects of the transaction have been made visible to another node. If a transaction TX is connected to TE T, it does not need to wait for the resolution of the lock if it does not modify any locked resource.

Similarly to a programming language mutex, once a SHARED or EXCLUSIVE lock has been acquired, the transaction needs to verify that no change happened since the beginning of the transaction. This is simple for READ COMMITTED transactions, since they do not guarantee a consistent snapshot across statements. An RC transaction will acquire all required SHARED locks, wait for any EXCLUSIVE locks that might have been acquired in the past and their effects; only then does it freeze it’s visibility snapshot. This ordering guarantees that the statement will see the newest, correct, and consistent snapshot of the database.

But what about CONSISTENT READ transactions? When using CR, the snapshot is frozen in time and can not be updated. This means that if a CR encounters an EXCLUSIVE lock, it might need to abort. If the EXCLUSIVE operation committed, this might mean that the view of the database is no longer compatible with the CR transaction. If it rolled back or failed, the CR can continue. If a CR can not continue you will see the exception ‘Table has been changed’. NuoDB only aborts transactions that have been proven to contain conflicting updates.

As I explained above, a transactional lock is similar to a mutex and should be treated that way. When using them, we recommend the check;lock;check pattern. Due to the tricky nature of snapshot visibility, NuoDB prohibits the use of the LOCK statement in CONSISTENT READ transactions. Since both check statements in the check;lock;check pattern would be executed with the same snapshot, they are guaranteed to return the same results. Any update that happened strictly before the EXCLUSIVE access was granted to the resource would not be reflected in the second check. To protect the application developer from such a mistake, NuoDB does not allow the LOCK statement in consistent read isolation level.

### USING LOCKS TO PREVENT WRITE SKEW

Write skew is a well known anomaly in MVCC that we might cover in future articles. For a quick primer...

A table contains three employees Bob, Mary, and Sue with their respective salaries:

| Employee | Salary |
| --- | --- |
| Bob | 100 |
| Mary | 150 |
| Sue | 70 |

Since the company had a good quarter, the CEO now opens a new req AND approves a salary increase for all the existing employees. The total cost can not be higher than 500.

The department head of department D1 looks at the total salary, calculates how much more the employees can be payed, and divides the difference equally. In SQL, that would look like:

SQL**\>** **select** **sum****(**salary**)** **as** "Existing Salaries" from employees;
 
 Existing Salaries  
 ------------------ 
 
        320   
SQL**\>** update employees **set** salary = salary + **(****select** **(**500-sum**(**salary**)****)****/**count**(**salary**)** from employees**)** ;
SQL**\>** **select** **\*** from employees;
 
 NAME  SALARY  
 ----- ------- 
 
 Bob     160   
 Mary    210   
 Sue     130   
 
SQL**\>** **select** **sum****(**salary**)** from employees;
 
 SUM  
 ---- 
 
 500  

Concurrently, the department head of D2 adds a new employee:

SQL**\>** **select** **sum****(**salary**)** from employees;
 
 SUM  
 ---- 
 
 320  
 
SQL**\>** insert into employees values**(**'Chung', 500-**(****select** **sum****(**salary**)** from employees**)****)**;
SQL**\>** **select** **\*** from employees;
 
 NAME  SALARY  
 ----- ------- 
 
 Bob     100   
 Mary    150   
 Sue      70   
 Chung   180   
 
SQL**\>** **select** **sum****(**salary**)** from employees;
 
 SUM  
 ---- 
 
 500

As we can see, these two transactions do not conflict in MVCC. Once both transactions commit, the CEOs limit is violated.

SQL**\>** **select** **sum****(**salary**)** from employees;
 
 SUM  
 ---- 
 
 680 

To prevent this from happening, one of the transactions can acquire an EXCLUSIVE lock. Let us look at the second transaction with locking. We will be using the check;lock;check pattern as explained in the introduction.

SQL**\>** start transaction isolation level **read** committed;
SQL**\>** **select** **sum****(**salary**)** from employees;
 
 SUM  
 ---- 
 
 320  
 
SQL**\>** lock table employees; **//** blocks **until** T1 resolves
SQL**\>** **select** **sum****(**salary**)** from employees;
 
 SUM  
 ---- 
 
 500  
SQL**\>** **//** Chung can not be hired

### UNDER THE HOOD

So how is this logic implemented? NuoDB does not use majority consensus algorithms, such as Paxos or Raft; instead, NuoDB depends on [Chairmanship](https://www.nuodb.com/techblog/why-chairman-aint-master-replica). All EXCLUSIVE requests will need to be granted by the chairman to ensure strict ordering. If an EXCLUSIVE request gets denied, the transaction will have to wait for the current holder to resolve before it can retry.

EXCLUSIVE locks use a form of INTENT. The chairman broadcasts an INTENT to acquire an EXCLUSIVE lock. All other engines will reply with a collection of all current SHARED locks.

Once all SHARED locks have been resolved, the INTENT is promoted to an EXCLUSIVE lock. Once an INTENT has been placed, no further SHARED or EXCLUSIVE locks can be placed. The promotion from INTENT to EXCLUSIVE is done without any additional messaging.

![Figure 1. Lifecycle and messaging involved in Transactional Locks](images/distributed-lock-techblog_fig1.png)

_Figure 1. Lifecycle and messaging involved in Transactional Locks_

The reliable broadcast protocol that NuoDB uses guarantees that the INTENT is either placed everywhere or nowhere. If the chairman dies during the locking protocol, the requestor will wait for the election of a new chairman before retrying.

Due to the use of INTENT-like locks, it is impossible to starve an EXCLUSIVE requestor. On the other hand, an incomplete EXCLUSIVE request will block future SHARED locks long before the requestor can take advantage of the lock. NuoDB assumes that an EXCLUSIVE lock is a rare operation and that application developers understand the cost of a distributed cluster-wide exclusive access. Long running SHARED lock owners can lock out everybody else from the resource for a long period of time. If you notice that taking an EXCLUSIVE lock takes a long period of time, consider consulting NuoDB pseudo table system.transactionallocks for debugging information.

![Figure 2. Long running SHARED lock prevents any action on the table](images/distributed-lock-techblog_fig2.png)

_Figure 2. Long running SHARED lock prevents any action on the table_

In **Figure 2** above we can see a situation when a long running analytical query by USER 1 owns a SHARED lock on table T1. While that transaction is in progress, no EXCLUSIVE lock on that resource can be acquired. When a DBA attempts to alter table T1 (which requires EXCLUSIVE access), her transaction will place an INTENT and block on the existing long running transaction. Once an INTENT has been placed, no further SHARED locks can be acquired. All transactions that already own a lock are allowed to proceed, but no new transactions can acquire the lock until the INTENT has been resolved. In this scenario, we can see that USER 2 is unable to insert into the table.

Here is the information that you can acquire from system tables in the situation described in **Figure 2**.

SQL**\>** **select** **\*** from system.transactionallocks;
 
 OBJECTID  TRANSID  NODEID  LOCKTYPE  SOURCENODE  
 --------- -------- ------- --------- ----------- 
 
    72       1410      2    Exclusive      2      
    72       1026      2    Shared         2      
SQL**\>** **select** **id**,state, blockedby from system.transactions;
 
  ID  STATE  BLOCKEDBY  
 ---- ------ ---------- 
 
 1026 Active      -1    
 1410 Active    1026    
 1666 Active    1410   

To let the system proceed, one of the actors in the dependency graph will need to resolve their transaction (commit, roll back, or fail).

### WHAT IS THE COST OF A LOCK?

The following section assumes that there are no conflicts - that is none of the requests hit a conflicting lock that is already in place. If there is a conflict, the lock request will have to wait for the resolution of the lock, and hence the transaction that owns that lock. Since the lifetime of any arbitrary transaction is outside the control of NuoDB, we will take that out of the equation.

Acquiring a SHARED lock (done automatically by NuoDB) is zero overhead. We have verified this by various in-house performance benchmarks, [TPC-C](http://www.tpc.org/tpcc/default.asp), and [YCSB](https://github.com/brianfrankcooper/YCSB).

The cost of acquiring an EXCLUSIVE lock is three times the network latency.

- Requestor -> Chairman (network latency in ms)
- Chairman -> all other nodes (network latency in ms)
- All other nodes -> requestor (latency in ms)

Since the requestor needs to receive a grant from every single TE in the system, the network latency of TEs located in remote data centers can be the major decisive component.

![Figure 3. Network latency and cost of a lock](images/distributed-lock-techblog_fig3.png)

_Figure 3. Network latency and cost of a lock_

Each lock is held until the end of its transaction. If a transaction contains multiple statements working on different resources, all those resources will be locked. Keep this in mind when you are performing large schema changes.

### SUMMARY

We have learned that expensive EXCLUSIVE transactional locks are not being used for normal CRUD operations. A CUD operation can operate without explicit approval from all nodes in the cluster by using a SHARED lock. An EXCLUSIVE lock is only required for certain metadata modifications and for special application-level use cases.

You can acquire an EXCLUSIVE lock for any type of operation that requires exclusive access to your table. It is possible to use EXCLUSIVE lock to work around write skew anomalies, but the recommended approach is to cause a write-write conflict by adding an additional void update on a well known record.

NuoDB does not use consensus algorithm, but instead depends on chairmanship for lock resolution.

This article first appeared at the [NuoDB Tech Blog](https://www.nuodb.com/techblog/quick-dive-nuodb-architecture) under the name [Distributed Transactional Locks](https://www.nuodb.com/techblog/distributed-transactional-locks)
