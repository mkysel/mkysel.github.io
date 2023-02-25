---
title: "Why A Chairman Ain't the Master Replica"
date: "2018-08-01"
categories: 
  - "nuodb"
---

### INTRO

As a distributed, ACID-compliant database, we get a lot of questions about how NuoDB works, and how it compares to more well-known architectures. In particular, people want to understand how NuoDB compares to a master / master architecture, and (related) how we handle lock management. This post will discuss both. We will also discuss when shared-nothing architecture is viable and when conflicting decisions need to coordinated.

In this blog post, we will discuss the concept of Chairmanship, NuoDB's answer to distributed coordination. We will prove that Chairmanship is both consistent and resilient to failure. But before we go into the details, let us explore the basics of NuoDB architecture.

### THE DEVIL IS IN THE ATOMS

To understand the role of Chairmen in NuoDB, we must first dive into the building blocks of the database. Under the hood, NuoDB splits the database into small chunks called “Atoms.” Each atom represents a relatively small portion of the overall data (64K). That could be  a series of rows in a table, one unique sequence, or a portion of a unique index.

Atoms are self-replicating and can exist simultaneously in multiple nodes of the database. To preserve consistency, some form of lock management and control is required.

NuoDB also allows for [data partitioning](https://www.nuodb.com/techblog/table-partitioning-and-storage-groups) (or sharding) where each node accesses a separate subset of all Atoms. As we all know, sharding the data right is an art form of its own. What happens when multiple nodes need to access the same data that spans multiple storage groups? How do we handle the situation when some of these accesses are writes?

### NOT ALL OPERATIONS REQUIRE COORDINATION

Since all nodes can actively serve client requests for both reading and writing  any given Atom, the system is more similar to master:master replication than it is to master:slave. Master:master _replication without coordination_ is the domain of eventually consistent systems and has many strategies (such as Last-Write-Wins, Push-Conflict-To-User) that do not apply to ACID databases and therefore to NuoDB.

Chairmanship is tightly coupled with the logic of MVCC. Before reading on, I would urge the reader to familiarize themselves with the concept. Check out this series [explaining MVCC](https://www.nuodb.com/techblog/mvcc-part-1-overview).

With MVCC, each node can read data without requiring coordination across nodes or locking. By definition in MVCC, transactions in snapshot isolation can not see modifications that were introduced after the transaction started. As a result, changes made on another node after a transaction has been started will not affect the reading of that data on the current node. The same holds true for non-conflicting writes, such as inserts into a table without a unique index. So far so good, no need for coordination with Chairmanship anywhere.

### DISTRIBUTED COORDINATION IS DONE ON THE CHAIRMAN

The real challenge occurs if distributed coordination is required. In this post, we are focusing on inserts into a table with a _unique index_, but there are many other decisions that need to be coordinated. Every time a transaction attempts to insert data into a unique index, a placeholder is inserted first. The placeholder is not readable yet, but prevents the insertion of duplicate entries. The request for such a blocking action goes to the Chairman (master/primary) of the respective data fragment. If there is no conflict, the requesting transaction has now successfully inserted a placeholder. All other transactions that want to insert the same data require an acknowledgement from the same Chairman to proceed. Since the placeholder already exists, such an acknowledgement won’t be granted. Once the transaction is committed, the placeholder turns into a row and becomes visible to other transactions.

At any given moment, each Atom only has one Chairman across the system, or the system is actively electing a new Chairman. We’ll plan to discuss how a Chairman is elected in a future post, but broadly, it’s related to the access pattern for that Atom’s data across all nodes. The Chairman will reside on one of the Transaction Engines serving the Atom.

### THE USE CASE OF CHAIRMANSHIP IN DETAIL

Let’s dig deeper into how a Chairman makes decisions and how the decisions survive failures. In this example, a client is connected to Transaction Engine 1 (TE1) and executes the following SQL statement:

`SQL> insert into table_A values (5);`

The table was defined as:

`SQL> create table table_A(i int unique);`

\[caption id="" align="aligncenter" width="701"\]![Figure 1: Atoms loaded in memory across the system](images/te-atom-diagram.png) Figure 1: Atoms loaded in memory across the system\[/caption\]

 

In Figure 1, there are three Transaction Engines (marked as TE1-3), each with a different subset of Atoms. For simplicity, let’s say each Atom represents a range of 100 elements in a unique index. The Chairman of Atom 1 is marked with a golden circle. To avoid confusion in this diagram, we are not showing which Atoms are the Chairman for the other Atoms. You might notice that there is no Atom 2. If the data in Atom 2 has not been recently accessed, it will eventually get garbage collected.

Let us examine a scenario where two transactions running on different engines try to insert the same row ([Figure 2](https://www.nuodb.com/techblog/why-chairman-aint-master-replica#fig2)). A transaction running on Transaction Engine 1 (TE1) is attempting to insert value 5. Since value 5 is in the 0-100 range for Atom 1, the insert will target that Atom. The Chairman for Atom 1 is TE2. TE1 and TE3 also have Atom 1 loaded in memory, but they are not the Chairman. They can resolve many decisions locally, but inserting into the unique index is not one of them. To make the insert globally coordinated, TE1 must contact TE2. In a trivial example, the insert is not in conflict with any previous insert (visible or invisible) and TE2 grants the request. The decision is broadcast to all engines serving the atom.

Concurrently, TE3 is trying to insert the same record, but the request arrives to TE2 late and it loses the uniqueness race. The Chairman TE2 declines the request and lets TE3 know.

\[caption id="" align="aligncenter" width="708"\]![Figure 2: Message flow in case of conflicting inserts](images/sequence-diagram_conflicting-inserts.png) Figure 2: Message flow in case of conflicting inserts\[/caption\]

 

Once the change from TE1 reaches TE3, it no longer needs to contact the Chairman. It knows that a placeholder is in flight and it can resolve future conflicts locally. The example in Figure 2 concludes with the transaction being committed.

### CHAIRMANSHIP DECISIONS ARE CONSISTENT IN CASE OF FAILURES

A savvy technical reader might ask themselves “that is good, but what happens if TE2 fails?”. Let us explore how Chairmanship failover works.

\[caption id="" align="aligncenter" width="656"\]![Figure 3: Message flow in case of Chairman failure](images/sequence-diagram_chairman-failover.png) Figure 3: Message flow in case of Chairman failure\[/caption\]

 

The Chairman does not need to reach consensus or get approval from other nodes before acknowledging the request. The decision can be made purely based on the local state of the engine. Since there can only be one Chairman at a time, there can be no conflicting decision in flight that the Chairman does not know about.

Once the Chairman has decided locally whether to accept or decline the request, it will broadcast the decision to every node serving the Atom. The reliable broadcast protocol guarantees that in case of a Chairman failure, the message makes it to every other node and therefore to the next Chairman. The guarantee is that each message is either known to no one or everyone. It is possible that both the requesting TE1 and the Chairman TE2 die at the same time and the message is never received by any other node. Since no one knows about the decision, it is as if it never happened. Once rebroadcast finished, all nodes know about all previous decisions and a new Chairman can be elected. Using NuoDB’s reliable broadcast protocol was preferred to more complicated consensus protocols. You can see the sequence diagram in Figure 3.

### CHAIRMANSHIP ALLOWS FINE-GRAINED DISTRIBUTION OF LOAD

Being a Chairman is not a stable state. Chairmen do not exist independently of their atoms.

Keep in mind that atoms are relatively small, and we assume that not all Transaction Engines have all the atoms loaded in memory. Once the atom is not used for a period of time, or if the Transaction Engine is getting close to its defined memory limit, it will be garbage collected from memory. If there is no atom, there is no Chairman. NuoDB chairmanship operations perform best if the atom access pattern across TEs is variable, and therefore, the shared overlap is as small as possible. Since all atoms have their Chairman, the number of these independent decision makers is relatively large. It is important to emphasize that not all Chairmen reside on the same engine, but are instead spread throughout the system. This leads to an important performance tradeoff in the system, the Engine that contains the Chairman for a given atom will have local access to the lock manager for an atom, giving it faster access to locking decisions. Other peers will have slower access to locking decisions.

While rebroadcast and Chairman election are running, no decision can be made. Decisions on other atoms can still be made by Chairmen that reside on other engines, but Atom 1 has no point of authority for the period of time it takes for a new Chairman to be elected. All transactions attempting to insert data into the fragment, will block until a new Chairman is elected.

### CONCLUSION

NuoDB distributes uniqueness management throughout the system in two ways:

1. The responsibility of managing uniqueness information and coordinating insert requests is distributed among multiple nodes in the system. This avoids putting a hot spot in the system where all management takes place.
2. Distributed uniqueness management is failure tolerant, improving system availability. Systems with a notion of centralized management may also achieve this by replicating the manager, but NuoDB does not require replicating special dedicated system resources for this, simplifying system administration.

This article first appeared at the [NuoDB Tech Blog](https://www.nuodb.com/techblog/quick-dive-nuodb-architecture) under the name [Why A Chairman Ain't the Master Replica](https://www.nuodb.com/techblog/why-chairman-aint-master-replica)
