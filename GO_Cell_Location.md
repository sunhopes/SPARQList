# GO cellular compartment ontology

## Endpoint
http://path-virtuoso:8890/sparql


## `list`

```sparql

PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX oboInOwl: <http://www.geneontology.org/formats/oboInOwl#>

SELECT DISTINCT ?cell_compart_id  str(?cell_comp_name) as ?cell_compart_name
FROM <http://localhost:8890/go_ontology>
WHERE {
   ?c_compart_id rdfs:subClassOf obo:GO_0005575 .
   ?cell_compart_id rdfs:subClassOf* ?c_compart_id;
                    rdfs:label ?cell_comp_name .      
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      
      return{
        "cell_comp_id": row.cell_compart_id.value.replace("http://purl.obolibrary.org/obo/", ""),
        "cell_comp_name": row.cell_compart_name.value
      }
    });
  }
})
```

## Comment
GO cellular compartment ontology