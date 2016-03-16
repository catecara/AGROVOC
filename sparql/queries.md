# Some reusable SPARQL queries for AGROVOC

## agrontology 

### Get all agrontology

select * { ?a a owl:ObjectProperty  } 

## Counts on properties

### Concepts with at least 2 instances of the same property

select ?s ?o1 ?o2
where {?s skos:definition ?o1.
       ?s skos:definition ?o2
      FILTER (?o1 != ?o2) } 
