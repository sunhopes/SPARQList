# pathway_pathwayStep_upload

## Parameters
* `nextstep_id` next pathwayStep number for pathway stepProcess 
* `pw_id` pathway id

## Endpoint
http://path-virtuoso:8890/sparql

## `input_table` 

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX : <http://glycosmos.org/biopax/pathway#>
INSERT DATA
{
    #GRAPH <http://localhost:8890/proteinPathwayUpload>
    #GRAPH <http://localhost:8890/testSpace>
    GRAPH <http://localhost:8890/testSpace2>      
    { 
      :{{pw_id}}_GC-PathwayStep{{rxn_id}} bp:nextStep :{{pw_id}}_GC-PathwayStep{{nextstep_id}} .
    }
     
 }

```
 ## Output

```javascript
({
  json({input_table}) {
    return input_table.results.bindings
    }
})
```   