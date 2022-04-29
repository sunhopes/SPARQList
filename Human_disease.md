# Human Disease

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?disease_id ?disease_name
FROM <http://localhost:8890/human_disease_ontology>
WHERE {
    ?child rdfs:subClassOf*  obo:DOID_4.
    ?disease_id rdfs:subClassOf ?child .
    ?disease_id rdfs:label ?disease_name .
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      // let array = row.pdb_ids.value.split(',')
      return{
        "Disease_id": row.disease_id.value.replace("http://purl.obolibrary.org/obo/",""),
        "Disease_name": row.disease_name.value
      }
    });
  }
})

```

## Comment
Human disease ontology in Bioportal