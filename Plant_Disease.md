# Plant Disease

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX obo: <http://purl.obolibrary.org/obo/>
SELECT DISTINCT ?disease_id ?disease_name
FROM <http://localhost:8890/plant_disease_ontology>
WHERE {
     ?parent rdfs:subClassOf* obo:PDO_0000001 .
     ?disease_id rdfs:subClassOf ?parent .
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
Plant disease ontology in POwiki