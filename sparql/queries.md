# Some reusable SPARQL queries for AGROVOC

## Indexing - AGRIS

### Extract Agrovoc concepts used in AGRIS, grouped by concept

prefix skosxl: <http://www.w3.org/2008/05/skos-xl#>    

select ?concept (count(?concept) as ?pCount)     
where {?doc dct:subject ?concept .    
            filter regex (str(?concept), "agrovoc")   
}   
group by ?concept ORDER BY DESC(?pCount)   
LIMIT 10000   


## Languages 

### Selects concepts with prefLabel beginning with ""

PREFIX skos:<http://www.w3.org/2004/02/skos/core#>    
SELECT distinct ?uri ?xlablit     
WHERE     
       { ?uri skos:prefLabel ?xlablit . FILTER( (regex(str(?xlablit), "^insects", "i")) && (lang(?xlablit)= "en")) }     
ORDER BY DESC(?xlablit) 


### Selects concepts with labeles in Lao and English

SELECT ?concept ?stringlo ?stringen    
WHERE {    
       ?concept <http://www.w3.org/2008/05/skos-xl#altLabel> ?xlabello  .    
       ?xlabello <http://www.w3.org/2008/05/skos-xl#literalForm> ?stringlo .     
       FILTER (lang(?stringlo ) ="lo") .     
       ?concept <http://www.w3.org/2008/05/skos-xl#altLabel> ?xlabelen  .    
       ?xlabelen <http://www.w3.org/2008/05/skos-xl#literalForm> ?stringen .    
       FILTER (lang(?stringen) ="en") .     
}

### Extract a linguistic version of Agrovoc (here FR and EN)

Construct { ?sConcept ?p ?o . ?o ?p2 ?o2 }     
where {    
	{   
		?father skos:topConceptOf <http://aims.fao.org/aos/agrovoc> .   
  		?sConcept skos:broader* ?father .     
 		?sConcept ?p ?o .     
   		FILTER((!isLiteral(?o) || lang(?o) = '' || langMatches(lang(?o), 'fr') || langMatches(lang(?o), 'en')) &&  
  		(isLiteral(?o) || !CONTAINS(str(?o), 'xl') || CONTAINS(str(?o), 'xl_fr') || CONTAINS(str(?o), 'xl_en')) ) .   			
 		OPTIONAL {   
 			?o ?p2 ?o2 .   
			FILTER((!isLiteral(?o2) || lang(?o2) = '' || langMatches(lang(?o2), 'fr') || langMatches(lang(?o2), 'en')) &&  (isLiteral(?o2) || !CONTAINS(str(?o2), 'xl') || CONTAINS(str(?o2), 'xl_fr') || CONTAINS(str(?o2), 'xl_en')) ) . 
				}    
	}   
}


## RDF vocabularies  

### Get all agrontology used in the data

select * { ?a a owl:ObjectProperty  } 

### Get SKOS properties used in the data

SELECT distinct ?pred     
WHERE {?x ?pred ?y .   
      filter regex (str(?pred), "skos")   
      }    



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


