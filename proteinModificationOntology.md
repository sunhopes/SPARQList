# Protein Modification Ontology (PSI_MOD)

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX mod: <http://purl.obolibrary.org/obo/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?mod_id ?mod_name
FROM <http://localhost:8890/protein_modification_onto>
WHERE {
    ?id rdfs:subClassOf mod:MOD_00000 .
    ?mod_id rdfs:subClassOf* ?id ;
     		rdfs:label ?mod_name .
   }

```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      // let array = row.pdb_ids.value.split(',')
      return{
        "PMOD_id": row.mod_id.value.replace("http://purl.obolibrary.org/obo/",""),
        "PMOD_name": row.mod_name.value.replace("<http://www.w3.org/2001/XMLSchema#string>","")
      }
    });
  }
})

```

## Comment
PSI-MOD ontology