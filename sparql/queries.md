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
      

### Count the number of occurrences of a given property by concept 

SELECT ?s (count(?s) as ?count)    
       WHERE{     
          ?s skos:definition ?o .     
       }    
       GROUP BY ?s 
       
### Coutns the concepts with > x instances of the same property

SELECT ?subj ?count   
WHERE{    
    FILTER(?count > 4)   
    {SELECT ?subj (count(?subj) as ?count)   
       WHERE{   
          ?subj skos:definition ?obj .   
       }   
       GROUP BY ?subj   
       
    }
}


## Concepts      
      
### Given a concept, all its properties

SELECT ?pred ?obj {    
<http://aims.fao.org/aos/agrovoc/c_208> ?pred ?obj     
}  


