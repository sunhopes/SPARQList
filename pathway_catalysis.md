<<<<<<< HEAD
# pathway_caltalysis_enzyme_upload

## Parameters
* `pw_id` pathway id
* `control_action` controller action value(activation or inhibition)
=======
# pathway_caltalysis_upload

## Parameters
* `EC_enzyme_id`  recognition of EC enzyme number
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
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
<<<<<<< HEAD
  GRAPH <http://localhost:8890/testSpace>  
  ##GRAPH <http://localhost:8890/proteinPathwayUpload>
    {    
		:{{pw_id}}_GC-Catalysis{{catalysis_node_num}} rdf:type bp:Catalysis ;
            	bp:controlled :{{pw_id}}_GC-BiochemicalReaction{{catalysis_node_num}} ;
            	bp:controller :{{pw_id}}_GC-{{catalysis_controller}} .
      
        {{#if control_action}}
         	:{{pw_id}}_GC-Catalysis{{catalysis_node_num}} bp:controlType "{{control_action}}"^^xsd:string .
      	{{/if}}
      
   		:{{pw_id}}_GC-PathwayStep{{catalysis_node_num}} rdf:type bp:PathwayStep ;
            	bp:stepProcess :{{pw_id}}_GC-Catalysis{{catalysis_node_num}} .
      
=======
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
    
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
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