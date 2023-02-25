---
title: "RDF parse of DBPedia showing all Death Metal band members and their connections"
date: "2016-03-02"
---

## Death Metal members and their connections

To anyone who likes Death Metal and would like to know who played with who in a graphical form. For you there is a DBpedia RDF parse that I've done over the last two days. Recently I replaced the PDF by a vector graphic to allow better scaling. I suggest you view the image in a new tab or [download](http://www.martinkysel.com/wp-content/uploads/2016/06/Death.svg) it. Also notice that bands have the format db:BAND\_NAME.

![Death Metal Parse](http://www.martinkysel.com/wp-content/uploads/2016/06/Death.svg)

## Tutorial

You can download the data as RDF or JSON from DBpedia using SPARQL.

```sql
SELECT 
  * 
WHERE 
  { {?band_uri dbo : genre dbr : Death_metal } 
UNION 
  {?band_uri dbo : genre dbr : Melodic_death_metal} 
UNION 
  {?band_uri dbo : genre dbr : Folk_metal} 
UNION 
  {?band_uri dbo : genre dbr : Pagan_metal} 
UNION 
  {?band_uri dbo : genre dbr : Black_metal} 
UNION 
  {?band_uri dbo : genre dbr : Viking_metal} 
UNION 
  {?band_uri dbo : genre dbr : Gothic_metal} 
UNION 
  {?band_uri dbo : genre dbr : Power_metal}.{?band_uri dbpedia2 : currentMembers ?member_uri} 
UNION 
  {?band_uri dbpedia2 : pastMembers ?member_uri}.}
```


I prefer a simple script in R
```bash
library(SPARQL)
library(igraph)
library(network)
library(ergm)
 
endpoint <- "<a class="vglnk" href="http://live.dbpedia.org/sparql" rel="nofollow"><span>http</span><span>://</span><span>live</span><span>.</span><span>dbpedia</span><span>.</span><span>org</span><span>/</span><span>sparql</span></a>"
options <- NULL
prefix <- c("db","<a class="vglnk" href="http://dbpedia.org/resource/" rel="nofollow"><span>http</span><span>://</span><span>dbpedia</span><span>.</span><span>org</span><span>/</span><span>resource</span><span>/</span></a>")
sparql_prefix <- "PREFIX owl: <<a class="vglnk" href="http://www.w3.org/2002/07/owl#" rel="nofollow"><span>http</span><span>://</span><span>www</span><span>.</span><span>w3</span><span>.</span><span>org</span><span>/</span><span>2002</span><span>/</span><span>07</span><span>/</span><span>owl</span><span>#</span></a>>
PREFIX xsd: <<a class="vglnk" href="http://www.w3.org/2001/XMLSchema#" rel="nofollow"><span>http</span><span>://</span><span>www</span><span>.</span><span>w3</span><span>.</span><span>org</span><span>/</span><span>2001</span><span>/</span><span>XMLSchema</span><span>#</span></a>>
PREFIX rdfs: <<a class="vglnk" href="http://www.w3.org/2000/01/rdf-schema#" rel="nofollow"><span>http</span><span>://</span><span>www</span><span>.</span><span>w3</span><span>.</span><span>org</span><span>/</span><span>2000</span><span>/</span><span>01</span><span>/</span><span>rdf</span><span>-</span><span>schema</span><span>#</span></a>>
PREFIX rdf: <<a class="vglnk" href="http://www.w3.org/1999/02/22-rdf-syntax-ns#" rel="nofollow"><span>http</span><span>://</span><span>www</span><span>.</span><span>w3</span><span>.</span><span>org</span><span>/</span><span>1999</span><span>/</span><span>02</span><span>/</span><span>22</span><span>-</span><span>rdf</span><span>-</span><span>syntax</span><span>-</span><span>ns</span><span>#</span></a>>
PREFIX foaf: <<a class="vglnk" href="http://xmlns.com/foaf/0.1/" rel="nofollow"><span>http</span><span>://</span><span>xmlns</span><span>.</span><span>com</span><span>/</span><span>foaf</span><span>/</span><span>0</span><span>.</span><span>1</span><span>/</span></a>>
PREFIX dc: <<a class="vglnk" href="http://purl.org/dc/elements/1.1/" rel="nofollow"><span>http</span><span>://</span><span>purl</span><span>.</span><span>org</span><span>/</span><span>dc</span><span>/</span><span>elements</span><span>/</span><span>1</span><span>.</span><span>1</span><span>/</span></a>>
PREFIX : <<a class="vglnk" href="http://dbpedia.org/resource/" rel="nofollow"><span>http</span><span>://</span><span>dbpedia</span><span>.</span><span>org</span><span>/</span><span>resource</span><span>/</span></a>>
PREFIX dbpedia2: <<a class="vglnk" href="http://dbpedia.org/property/" rel="nofollow"><span>http</span><span>://</span><span>dbpedia</span><span>.</span><span>org</span><span>/</span><span>property</span><span>/</span></a>>
PREFIX dbpedia: <<a class="vglnk" href="http://dbpedia.org/" rel="nofollow"><span>http</span><span>://</span><span>dbpedia</span><span>.</span><span>org</span><span>/</span></a>>
PREFIX skos: <<a class="vglnk" href="http://www.w3.org/2004/02/skos/core#" rel="nofollow"><span>http</span><span>://</span><span>www</span><span>.</span><span>w3</span><span>.</span><span>org</span><span>/</span><span>2004</span><span>/</span><span>02</span><span>/</span><span>skos</span><span>/</span><span>core</span><span>#</span></a>>
"
q2 <- paste(sparql_prefix,
'SELECT *
WHERE {
{?band_uri dbo:genre dbr:Death_metal }
UNION {?band_uri dbo:genre dbr:Melodic_death_metal} 
UNION {?band_uri dbo:genre dbr:Folk_metal}
UNION {?band_uri dbo:genre dbr:Pagan_metal}
UNION {?band_uri dbo:genre dbr:Black_metal}
UNION {?band_uri dbo:genre dbr:Viking_metal}
UNION {?band_uri dbo:genre dbr:Gothic_metal}
UNION {?band_uri dbo:genre dbr:Power_metal}.
{?band_uri dbpedia2:currentMembers ?member_uri}
UNION {?band_uri dbpedia2:pastMembers ?member_uri}.
}')
res <- SPARQL(endpoint,q2,ns=prefix,extra=options)$results
 
member_band_matrix <- as.matrix(ifelse(table(res$member_uri, res$band_uri) > 0, 1, 0))
 
a_m <- graph.incidence(member_band_matrix)
 
write.graph(a_m,'death_members.graphml',format="graphml")
```

Afterwards you can use tools such as [Gephi](https://gephi.org/) to visualise the data.
