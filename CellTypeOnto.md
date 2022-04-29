# Cellular types of Cell Ontology

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX obo: <http://purl.obolibrary.org/obo/>

SELECT DISTINCT  ?child_id  (str (?child_name) as ?ctype_name) 
FROM <http://localhost:8890/celltype>
WHERE {
   ?child_id rdfs:subClassOf+ obo:CL_0000000 .
   ?child_id rdfs:subClassOf ?parent_id.
   ?child_id rdfs:label ?child_name.      
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      return{
        "cType_id": row.child_id.value.replace("http://purl.obolibrary.org/obo/", ""),
        "cType_name": row.ctype_name.value
        
      }
    });
  }
})

```

## Comment
Cell type name extraction from cell ontology.