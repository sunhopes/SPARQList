# Animal Disease

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX ando: <http://opendata.inra.fr/AnimalDiseasesOnto/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT DISTINCT ?disease_id ?disease_name
FROM <http://localhost:8890/animal_disease_ontology>
WHERE {
     ?parent rdfs:subClassOf* ando:Disease .
     ?disease_id rdfs:subClassOf ?parent .
     ?disease_id skos:prefLabel ?disease_name .
     FILTER (LANG(?disease_name) = 'en') .
}

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      // let array = row.pdb_ids.value.split(',')
      return{
        "Disease_id": row.disease_id.value.replace("http://opendata.inra.fr/AnimalDiseasesOnto/",""),
        "Disease_name": row.disease_name.value.replace("@en", "")
      }
    });
  }
})

```

## Comment
Animal disease ontology in AgroPortal