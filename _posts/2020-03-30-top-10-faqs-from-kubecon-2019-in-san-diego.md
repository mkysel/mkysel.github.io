---
title: "Top 10 FAQs from KubeCon 2019 in San Diego"
date: "2020-03-30"
categories: 
  - "nuodb"
tags: 
  - "featured"
---

## WHAT DOES NUODB DO?

TL;DR: NuoDB is a [distributed SQL database](https://www.nuodb.com/digging-distributed-sql). 

NuoDB is a SQL database, it is fully transactional, fully ACID, and fully consistent. This is a stark contrast to NoSQL solutions that have emerged in the last decade.

NuoDB is distributed, meaning that it is able to scale horizontally and automatically keeps multiple replicas in sync. Having multiple replicas of a database is a strict requirement for any service having 99.99% or better availability guarantees.

## DO YOU RUN IN THE CLOUD?

NuoDB runs on all public clouds (AWS, Azure, GCP, etc.), on private clouds, and on-prem. We run in [Kubernetes](https://www.nuodb.com/techblog/nuodb-golang-operator-now-available-delivering-automated-day-2-operations), [Docker](https://www.nuodb.com/techblog/deploy-nuodb-database-docker-containers-pt1), VMs, bare metal, you name it. NuoDB allows you full flexibility to install the product anywhere you want while avoiding vendor lock-in with any of the cloud providers. I call our ability to run anywhere cloud-agnostic. Which is different from the next buzz word... multi-cloud.

## DO YOU RUN IN MULTIPLE CLOUDS?

Being cloud-agnostic, we run in any of the clouds, but we also run in [multi-cloud](https://zoom.us/webinar/register/1815737622276/WN_m5tD0WPhRx6GOqK9bmcX5g). Given that multi-cloud is a loaded buzz word, let me explain what multi-cloud means to us. We give our customers the ability to run one logical NuoDB database across multiple clouds, such as AWS and Azure, at the same time. Read about the [KubeCon partnership announcement with Rancher](https://www.nuodb.com/company/press-releases/nuodb-partners-rancher-labs-deliver-cloud-native-sql-database-across-multi).

The ability to run in multiple clouds pushes the boundaries of zero-downtime even further. Having a single logical database that spans two or more clouds allows your service to continue running even in the case of serious cloud provider failures.

## WHAT ARE YOUR CONSISTENCY GUARANTEES?

Using the computer science term [CAP](https://en.wikipedia.org/wiki/CAP_theorem), NuoDB is a CP database, meaning that we prioritize consistency over availability. Once NuoDB acknowledges a transaction, it is guaranteed to be stored in the cluster following all [ACID](https://en.wikipedia.org/wiki/ACID) principles. NuoDB is well suited for high-value data.

## ARE YOU BUILT ON TOP OF ANOTHER DATABASE?

NuoDB is not build on top of another (open source) database such as [MySQL](https://www.mysql.com/) or [PostgreSQL](https://www.postgresql.org/). We have our own client, SQL engine, query planner, and query optimizer. As such, we have a slightly different SQL dialect from other SQL databases. We are compliant with the ANSI SQL standard. NuoDB has been around since 2010 and has had a chance to build our own custom SQL layer that is tuned to modern distributed environments.

## DO YOU USE CONSENSUS/RAFT?

The short answer is no. The longer answer is: yes, but only for configuration management as part of the administration tier. Consensus for data changes does not scale well, which is why databases that are based on it combine it with sharding. Even with sharding, performance suffers due to serializing writes (and reads) through each shard's leader and synchronizing with multiple leaders for cross-shard transactions.

## ARE YOU OPEN SOURCE?

No. NuoDB is not open source. We do provide a [Community Edition](https://www.nuodb.com/dev-center/community-edition-download) (CE) of our product that you can download today. To make things even easier, you can download our open source [Helm Charts](https://github.com/nuodb/nuodb-helm-charts) or [Go Operator](https://github.com/nuodb/nuodb-operator) and get started in Kubernetes. You can also find our operator in [Quay.io](https://quay.io/repository/nuodb/nuodb-operator), [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B07Z8D86BF?qid=1574450512463&sr=0-1&ref_=srh_res_product_title), [Google Cloud Platform](https://console.cloud.google.com/marketplace/details/nuodb/nuodb-operator?supportedpurview=project), [Red Hat Catalog](https://access.redhat.com/containers/?tab=overview#/registry.connect.redhat.com/nuodb/nuodb-operator), or [OperatorHub.io](https://operatorhub.io/operator/nuodb-operator-bundle). The CE is fully functional, but limits the scale-out to three Transaction Engines (TEs) and one Storage Manager (SM).

## HOW EASY IS IT TO MIGRATE TO NUODB?

Any database migration requires work. NuoDB is not a fork of another database and as such, we implement our own SQL dialect. While we have added SQL that is compatible to Oracle and SQL Server dialect, you may be using certain vendor-specific extensions that may need to be modified.

More importantly, migrating existing applications to NuoDB might be easier than migrating to other distributed SQL databases because we abstract the complexity of sharding away and make the initial transition into the cloud and/or into Kubernetes easier. What does that mean? Existing applications that have been written for traditional single-node SQL databases generally are not easily partitionable or shardable. As a result, migrating to a distributed SQL database that requires partitioning or sharding may be very difficult  for an existing application. NuoDB does support [sharding](https://www.nuodb.com/techblog/table-partitioning-and-storage-groups), but you won’t need it from day 0. NuoDB [Transaction Engines](https://www.nuodb.com/techblog/quick-dive-nuodb-architecture) act as a form of LRU cache that dynamically adapts to the application access patterns without the need for large-scale application rewrites. If you have a monolithic application that is running against an enterprise database today, you can migrate to the cloud first and start breaking the monolith down second.

## HOW DO YOU COMPARE TO COCKROACHDB/YUGABYTE?

While we are all distributed SQL databases, our architectural approach to achieve distribution is significantly different from the others, which results in different addressable use cases.

CockroachDB and Yugabyte both leverage automatic partitioning and a consensus algorithm to achieve distribution. In contrast, NuoDB’s architecture splits the compute and storage layers of a standard database into two different processes: Transaction Engines and Storage Managers. The Transaction Engines provide a form of LRU in-memory cache, which is coupled with Storage Managers for data persistence. Each layer can be scaled independently.

Our architectural approach allows our customers to migrate existing enterprise critical applications off Oracle, SQL Server, or DB2 more easily. As noted above, we don’t require a partitionable or shardable workload. Also, we don’t use a consensus algorithm for data management. As a result, we can address applications requiring very low latency and high transactional throughput. This was demonstrated in the [Temenos benchmark](https://www.nuodb.com/company/press-releases/temenos-benchmarks-its-cloud-native-digital-banking-software-aws-and-proves), where we were able to deliver over 50K TPS for core banking transactions.

## HOW DO I GET STARTED WITH NUODB?

Check out this straightforward tutorial, which will get you [up and running with NuoDB Community Edition](https://www.nuodb.com/techblog/scale-out-nuodb-community-edition). If you have more questions, [please contact us](https://www.nuodb.com/company/contact-us).

This article first appeared on the NuoDB [technical blog](https://www.nuodb.com/techblog/top-10-faqs-kubecon-2019-san-diego).
