# Some reusable SPARQL queries for AGROVOC

## agrontology 

### Get all agrontology

select * { ?a a owl:ObjectProperty  } 

## Properties count

### Concepts with at least 2 instances of the same property

SELECT ?s ?o1 ?o2  
WHERE {?s skos:definition ?o1.  
       ?s skos:definition ?o2  
      FILTER (?o1 != ?o2) } 
      
      
