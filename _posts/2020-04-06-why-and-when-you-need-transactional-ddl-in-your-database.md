---
title: "Why and When You Need Transactional DDL in Your Database"
date: "2020-04-06"
categories: 
  - "nuodb"
---

We typically talk about transactions in the context of Data Manipulation Language (DML), but the same principles apply when we talk about Data Definition Language (DDL). As databases increasingly include transactional DDL, we should stop and think about the history of transactional DDL. Transactional DDL can help with application availability by allowing you perform multiple modifications in a single operation, making software upgrades simpler. You’re less likely to find yourself dealing with a partially upgraded system that requires your database administrator (DBA) to go in and fix everything by hand, losing hours of their time and slowing your software delivery down.

## WHY DO YOU CARE?

If you make a change to application code and something doesn’t work, you don’t want to have to deal with a complicated recovery. You want the database to be able to roll it back automatically so you get back to a working state very rapidly. Today, very often people don’t write code for databases, they have frameworks that do it (Hibernate, for example). This makes it impossible for a software engineer to write the code properly, or roll it back, because they don’t work on that level. When you make changes using rolling application upgrades, it is simpler, less likely to fail, and more obvious what to do when you do experience a failure.

With transactional DDL, it’s far less likely that you will have to deal with a partially upgraded system that has essentially ground your application to a stop. Partial upgrades like that may require your database administrator (DBA) to go in and fix everything by hand, losing hours of their time and slowing your software delivery down. With transactional DDL, you can roll back to the last working upgrade and resolve the issues rapidly, without taking your software delivery system or your application offline.

## A SHORT EXPLANATION OF DML AND DDL

Essentially, DML statements are structured query language (SQL) statements that we use to manipulate data — as you might have guessed. Specifically, the DML class includes the INSERT, UPDATE, and DELETE SQL statements. Sometimes, we refer to these three statements as WRITE DML, and we call the SELECT statement READ DML. The standard does not differentiate between read and write, but for this article, it is an important distinction.

On the other hand, DDL is a family of SQL language elements used to define the database structure, particularly database schemas. The CREATE, ALTER, and DROP commands are common examples of DDL SQL statements, but DDL language elements may include operations with databases, tables, columns, indexes, views, stored procedures, and constraints.

Next, let’s start by defining a transaction as a sequence of commands collected together into a single logical unit, and a transaction is then executed as a single step. With this definition, if the execution of a transaction is interrupted, the transaction isn’t executed. Because a transaction must be ACID — Atomic, Consistent, Isolated, and Durable, that means that when a transaction executes several statements, some of which are DDL, it treats them as a single operation that can either be rolled back or committed. This means that you will never leave the database in a temporary, non-consistent state. Historically, databases haven’t provided the functionality of transactional DDL statements, but even today not all databases provide the functionality of truly transactional DDL. In most cases, this functionality comes with limitations.

Now, what does true “transactional DDL” mean? It means that all statements should be ACID, regardless of whether they are DML or DDL statements. In practice, with most databases, DDL statements break the transactionality of the enclosing transaction and cause anomalies.

## A BRIEF HISTORY OF DDL

Originally, the idea of a data definition language was introduced as part of the Codasyl database model. CODASYL is the Conference/Committee on Data Systems Languages, and was formed as a consortium in 1959 to guide development of a standard programming language, which resulted in COBOL, as well as a number of technical standards. CODASYL also worked to standardize database interfaces, all part of a goal from its members to promote more effective data systems analysis, design, and implementation.

In 1969 CODASYL’s Data Base Task Group (DBTG) published its first language specifications for their data model: a data definition language for defining the database schema, another DDL to define application views of the database, and (you guessed it) a data manipulation language that defined verbs to request and update data in the database. Later DDL was used to refer to a subset of SQL to declare tables, columns, data types, and constraints, and SQL-92 introduced a schema manipulation language and schema information tables to query schemas. In SQL:2003 these information tables were specified as SQL/Schemata.

Transactionality of DDL, however, is not part of the ANSI SQL standard. Section 17.1 of [ANSI SQL 2016](https://webstore.ansi.org/Standards/ISO/ISOIEC90752016-1646101?source=blog) () only specifies the grammar and the supported isolation levels. It does not specify how a transaction should behave or what ‘transactional’ means.

## WHY ISN’T TRANSACTIONAL DDL UNIVERSALLY PROVIDED?

There’s no reason why DDL statements shouldn’t be transactional, but in the past, databases haven’t provided this functionality. In part that’s because transactional DDL implies that DDL must happen in isolation from other transactions that happen concurrently. That means that the metadata of the modified table must be versioned. To correctly process metadata changes, we need to be able to roll back DDL changes that were aborted due to a transaction rollback. That’s not easy — in fact it’s a complex algorithmic task that requires the database to support metadata delta (diff), which corresponds to DDL changes within the current transaction of each connection to the database. This delta exists before the transaction is closed, and so it could be rolled back as a single transaction, or in parts in RDBMSs that support multi-level transactions or savepoints. Essentially, it’s not universally provided because it’s hard to do correctly.

> For organizations moving to microservices, DevOps, and CI/CD, they have an essential new requirement — a database that supports online transactional DDL.

Let’s return to our concept of WRITE DML (update, delete, insert) vs. READ DML. You might ask yourself how do these statements collide in a system that supports multiple concurrent transactions and a DDL transaction is ongoing? Ideally the set of transactions that collide is as small as possible. A SELECT statement does not collide with an INSERT statement. There is no reason why this should be any different in the context of DDL. A DDL statement ALTER TABLE should not prevent a SELECT statement from executing concurrently. This is a common pattern in database design.

For DDL to be transactional you need to support multiple versions concurrently. Similar to multiversion concurrency control (MVCC), readers don’t block writers and writers don’t block readers. Without MVCC it’s hard to have transactional DDL. Traditionally, databases started with a locking system instead of MVCC. That implementation wasn’t suited to transactional DDL, which is why around 2005 there was a big shift towards MVCC — to provide concurrent access to the database, and to implement transactional memory in programming languages.

MVCC provides the semantics we might naturally desire. Read DML can proceed while conflicting writes (write DML and DDL) are executed concurrently.

## WRITE DML AND DDL IN A LIVE SYSTEM

We have established that Read DML (SELECT) can happily proceed regardless of what else is executing concurrently in the system. Write DML (INSERT, UPDATE, DELETE) is not allowed to execute on a table that is being concurrently modified by DDL. Explaining the semantics of the conflicts expected behavior based on the Isolation Levels of all concurrent transactions is beyond the scope of this article.

To simplify the discussion, we state that both write DML and DDL are mutually exclusive if executed on the same resource. This results in operations blocking each other.  If you have a long-running DDL transaction, such as a rolling upgrade of your application, write DML will be prevented for a long period of time.

So even though the DDL is transactional, it can still lead to database downtime and maintenance windows. Or does it?

## ALWAYS ONLINE, ALWAYS AVAILABLE

The database industry is moving towards an always online, always available model. Earlier databases resulted in a message saying that something wasn’t available — that was essentially because you grabbed a lock in a database for a long period of time. That isn’t an option in an always online, always available model.

Customers, and therefore organizations, require applications to be online and available all the time. That means that transactional DDL is mandatory for the new world, and not only for the applications to run the way customers require them to. It’s also mandatory for organizations adopting DevOps and continuous integration and continuous delivery models (CI/CD). Specifically, that’s because without transactional DDL, applications developers cannot safely and easily make database schema changes (along with their app changes) online. That means that for organizations moving to microservices, DevOps, and CI/CD, they have an essential new requirement — a database that supports online transactional DDL.

I personally consider the term ONLINE misleading. The database is not offline while it holds a lock on a resource. A more appropriate term would have been LOCK FREE DDL. That is, metadata modification can happen without locking out concurrent DML.

## THE AVAILABILITY VS. SIMPLICITY TRADEOFF

We said that write DML cannot happen concurrently with DDL to avoid ACID violations. Now, what happens to a system that has to be always up and needs to execute long-running DDL statements? Luckily enough, most DDL statements do not take a long time. Adding or removing columns from a table takes a constant amount of time, regardless of how long the table is. If the set of changes is small enough, it is OK to lock DML out for a short period of time.

But there are some DDL statements that need to process every row in the table and hence it can take a long time to process large tables. CREATE INDEX is a prime example of a long-running statement. Given that index creation can take multiple hours, it is not an acceptable option for an always online, always available application.

Specifically, for index creation, NuoDB and other databases implement an ONLINE or CONCURRENT version (I would have preferred to call it a LOCK FREE version). This version allows DBAs to create indexes without locking the table or requiring a maintenance window — an extremely important capability in a 24×7 availability world. However, these capabilities do not come for free. Online versions of common DDL statements tend to be slightly slower than their LOCKING versions, have complicated failure modes, and hard to understand ACID semantics. They also cannot be part of a larger multistatement DDL transaction.

Interestingly enough, sometimes the speed of execution is not the primary concern. In which case you may not want to use the online version. You might have an application that requires complex changes to a database schema and some LOCKING is a viable tradeoff for a much simpler upgrade procedure.

Atomicity, one of the four guarantees of ACID, states that: _“Each transaction is treated as a single ‘unit,’ which either succeeds completely or fails completely.”_

This becomes a very desirable quality if we think of transactions as a set of many DDL statements. An example would be: alter a table; create a log table with a similar name; insert a few rows to other tables. We already know that if DDL is not treated transactionally, you could end up with new rows in the other tables, but neither the CREATE nor the ALTER succeeded. Or you could end up with just the CREATE and no ALTER.

So, if you have an application that assumes that if there is a log table (the CREATE) it can also assume that the ALTER has happened, you might run into subtle bugs in production if the upgrade did not fully complete. Transactional DDL gives database administrators the ability to perform multiple modifications (such as the example above) in a single operation.

For developers, the strong Isolation guarantees of transactional DDL makes the development of applications easier. An application can only observe the database in state A (before the upgrade) or in-state B (after the upgrade) and will never see partial results. This reduces the required test matrix and increases confidence in the rolling upgrade procedure. Now that is easy to code against.

The tradeoff between simplicity of rolling upgrades that is LOCKING and the always available, always online that is NOT TRANSACTIONAL has been known to the industry since InterBase introduced MVCC to the commercial market.

## CHOOSE TRANSACTIONAL DDL

Databases have changed a lot since 1959, and there have been many changes in customer expectations for user experience and application availability since then. Transactional DDL helps you avoid a scenario in which your application is no longer available, and gives your DBAs some peace of mind, knowing they won’t have to painstakingly repair the database to bring your software delivery back up to speed. Today, many databases offer transactional DDL, which will help you resolve immediate issues quickly by rolling back to the last working upgrade. In order to meet the always available requirements of today, choose a database that offers transactional DDL. But keep in mind that modern always-available, always-online applications require a database that not only simplifies upgrade scenarios, but also limits downtime due to long-running metadata modifications.

_[This article was originally published in The New Stack.](https://thenewstack.io/why-and-when-you-need-transactional-ddl-in-your-database/)_
