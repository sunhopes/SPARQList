# GlyCosmos Glycolipids List

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT DISTINCT  ?lipid_id  ?lipid_name   
FROM <http://localhost:8890/lipidMap>
WHERE{
    ?lipidmaps dcterms:identifier ?lipid_id .
    optional{
      ?lipidmaps rdfs:label ?lipid_name .}
    }
```

## Output

```javascript
({
  json({list}){
    return list.results.bindings.map((row) => {
      if(row.lipid_name){
        Lipid_Name = row.lipid_name.value;
      }else{
        Lipid_Name = "";
      }
      return{  
      	"backbone_id": row.lipid_id.value,
        "backbone_name": Lipid_Name  
      }
    });
  }
})

```

## Comment
Lipid list from LipidMap.