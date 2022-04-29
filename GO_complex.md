# GO protein containing complex

## Endpoint
http://path-virtuoso:8890/sparql


## `list`

```sparql

PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?complex_id  ?complex_name
FROM <http://localhost:8890/GO> 
WHERE {
   ?cell_complex_id rdfs:subClassOf obo:GO_0032991 .
   ?complex_id rdfs:subClassOf* ?cell_complex_id;
                    rdfs:label ?complex_name .      
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      
      return{
        "protein_complex_id": row.complex_id.value.replace("http://purl.obolibrary.org/obo/", ""),
        "protein_complex_name": row.complex_name.value
      }
    });
  }
})
```

## Comment
GO protein-containing complex