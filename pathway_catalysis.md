# pathway_caltalysis_upload

## Parameters
* `EC_enzyme_id`  recognition of EC enzyme number
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
    GRAPH <http://localhost:8890/proteinPathwayUpload>
    {    
	:GC-Catalysis{{catalysis_node_num}} rdf:type bp:Catalysis ;
            bp:controlled :GC-BiochemicalReaction{{catalysis_node_num}} ;
            bp:controller :GC-{{catalysis_controller}} ;
            bp:xref :GC-UnificationXref{{unixref_num_2}};
            bp:controlType "ACTIVATION"^^xsd:string .
      
    :GC-UnificationXref{{unixref_num_2}} rdf:type bp:UnificationXref ;
            bp:db "EC enzyme"^^xsd:string ;
            bp:id "{{EC_enzyme_id}}"^^xsd:string .
     
     :GC-PathwayStep{{catalysis_node_num}} rdf:type bp:PathwayStep ;
            bp:stepProcess :GC-Catalysis{{catalysis_node_num}} .
      
   {{#if EC_enzyme_id}}
     :GC-BiochemicalReaction{{catalysis_node_num}} bp:eCNumber  "{{EC_enzyme_id}}"^^xsd:string .
   {{/if}}  
    
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