## Endpoint

http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX obo: <http://purl.obolibrary.org/obo/>

SELECT  DISTINCT  ?child_id ?child_id_name ?real_child_id ?real_child_name  
FROM <http://localhost:8890/pathway_ontology>
WHERE {
     ?child_id rdfs:subClassOf* obo:PW_0000001.
     ?real_child_id rdfs:subClassOf ?child_id;
                  rdfs:label ?real_child_name.
     ?child_id rdfs:label ?child_id_name .
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      
      if(row.child_id_name.value == "pathway"){
          parent = "#";
     }else{
          parent = row.child_id.value.replace("http://purl.obolibrary.org/obo/","");
     }
      return{
        "id": row.real_child_id.value.replace("http://purl.obolibrary.org/obo/",""),
        "parent": parent,
        "text": row.real_child_name.value
      }
    });
  }
})

```

## Comment
Pathway Ontology classes for tree menu