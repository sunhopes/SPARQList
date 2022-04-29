# Brenda Tissue Ontology

## Endpoint

http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX obo: <http://purl.obolibrary.org/obo/>

SELECT DISTINCT  ?t_id ?t_name ?t_child_id ?t_child_name
FROM <http://8890/brendaTissue>
WHERE {
    ?t_id rdfs:subClassOf* obo:BTO_0000000 .
    ?t_child_id rdfs:subClassOf ?t_id .
    ?t_child_id rdfs:label ?t_child_name .
    ?t_id rdfs:label ?t_name .
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {

      if(row.t_name.value == "tissues, cell types and enzyme sources"){
          parent = "#";
     }else{
          parent = row.t_id.value.replace("http://purl.obolibrary.org/obo/", "");
     }
     return{
        "id": row.t_child_id.value.replace("http://purl.obolibrary.org/obo/", ""),
        "parent": parent,
        "text": row.t_child_name.value
      }
    });
  }
})

```

## Comment
Tissue ontology was extracted from Brenda Tissue Ontology.
