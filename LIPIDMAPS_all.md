# GlyCosmos Glycolipids List

## Endpoint
http://path-virtuoso:8890/sparql

## `list`
```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX sio: <http://semanticscience.org/resource/>

SELECT DISTINCT ?lipid_id ?common_name ?system_name
FROM <http://localhost:8890/lipidmaps_all>
WHERE {
    ?lipid_id rdfs:label ?system_name;
              sio:SIO_000118 ?common_name.
}
```

## Output

```javascript
({
  json({list}){
    return list.results.bindings.map((row) => {
      return{  
      	"backbone_id": row.lipid_id.value.replace("http://glycosmos.org/proteinpathway/lipidmaps/# ",""),
        "backbone_name": row.common_name.value,
        "backbone_alt_name": row.system_name.value
      }
    });
  }
})

```

## Comment
All kinds of lipid from LipidMaps (common name and systemic name).