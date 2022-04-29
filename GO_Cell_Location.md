# GO cellular compartment ontology

## Endpoint
http://path-virtuoso:8890/sparql


## `list`

```sparql

PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?cell_compart_id ?cell_comp_name 
FROM <http://localhost:8890/GO> 
WHERE {
   { ?c_compart_id rdfs:subClassOf obo:GO_0110165 .
     ?cell_compart_id rdfs:subClassOf* ?c_compart_id;
                      rdfs:label ?cell_comp_name .
     FILTER(!regex(str(?cell_comp_name), "complex")) 
   }
   
  UNION
   { ?c_compart_id rdfs:subClassOf obo:GO_0044423 .
     ?cell_compart_id rdfs:subClassOf* ?c_compart_id;
                      rdfs:label ?cell_comp_name .
   }
 }


```
## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      
      return{
        "cell_local_id": row.cell_compart_id.value.replace("http://purl.obolibrary.org/obo/", ""),
        "cell_local_name": row.cell_comp_name.value
      }
    });
  }
})
```

## Comment
GO cellular compartment ontology