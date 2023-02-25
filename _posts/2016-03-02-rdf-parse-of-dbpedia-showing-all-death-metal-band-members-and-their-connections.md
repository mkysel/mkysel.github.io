---
title: "RDF parse of DBPedia showing all Death Metal band members and their connections"
date: "2016-03-02"
---

## Death Metal members and their connections

To anyone who likes Death Metal and would like to know who played with who in a graphical form. For you there is a DBpedia RDF parse that I've done over the last two days. Recently I replaced the PDF by a vector graphic to allow better scaling. I suggest you view the image in a new tab or [download](http://www.martinkysel.com/wp-content/uploads/2016/06/Death.svg) it. Also notice that bands have the format db:BAND\_NAME.

![Death Metal Parse](http://www.martinkysel.com/wp-content/uploads/2016/06/Death.svg)

* * *

## Tutorial

You can download the data as RDF or JSON from DBpedia using SPARQL.

\[sql\]

SELECT \* WHERE { {?band\_uri dbo:genre dbr:Death\_metal } UNION {?band\_uri dbo:genre dbr:Melodic\_death\_metal} UNION {?band\_uri dbo:genre dbr:Folk\_metal} UNION {?band\_uri dbo:genre dbr:Pagan\_metal} UNION {?band\_uri dbo:genre dbr:Black\_metal} UNION {?band\_uri dbo:genre dbr:Viking\_metal} UNION {?band\_uri dbo:genre dbr:Gothic\_metal} UNION {?band\_uri dbo:genre dbr:Power\_metal}. {?band\_uri dbpedia2:currentMembers ?member\_uri} UNION {?band\_uri dbpedia2:pastMembers ?member\_uri}. } \[/sql\]

I prefer a simple script in R

\[splus\] library(SPARQL) library(igraph) library(network) library(ergm)

endpoint <- "http://live.dbpedia.org/sparql" options <- NULL prefix <- c("db","http://dbpedia.org/resource/") sparql\_prefix <- "PREFIX owl: <http://www.w3.org/2002/07/owl#> PREFIX xsd: <http://www.w3.org/2001/XMLSchema#> PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> PREFIX foaf: <http://xmlns.com/foaf/0.1/> PREFIX dc: <http://purl.org/dc/elements/1.1/> PREFIX : <http://dbpedia.org/resource/> PREFIX dbpedia2: <http://dbpedia.org/property/> PREFIX dbpedia: <http://dbpedia.org/> PREFIX skos: <http://www.w3.org/2004/02/skos/core#> " q2 <- paste(sparql\_prefix, 'SELECT \* WHERE { {?band\_uri dbo:genre dbr:Death\_metal } UNION {?band\_uri dbo:genre dbr:Melodic\_death\_metal} UNION {?band\_uri dbo:genre dbr:Folk\_metal} UNION {?band\_uri dbo:genre dbr:Pagan\_metal} UNION {?band\_uri dbo:genre dbr:Black\_metal} UNION {?band\_uri dbo:genre dbr:Viking\_metal} UNION {?band\_uri dbo:genre dbr:Gothic\_metal} UNION {?band\_uri dbo:genre dbr:Power\_metal}. {?band\_uri dbpedia2:currentMembers ?member\_uri} UNION {?band\_uri dbpedia2:pastMembers ?member\_uri}. }') res <- SPARQL(endpoint,q2,ns=prefix,extra=options)$results

member\_band\_matrix <- as.matrix(ifelse(table(res$member\_uri, res$band\_uri) > 0, 1, 0))

a\_m <- graph.incidence(member\_band\_matrix)

write.graph(a\_m,'death\_members.graphml',format="graphml") \[/splus\]

Afterwards you can use tools such as [Gephi](https://gephi.org/) to visualise the data.
