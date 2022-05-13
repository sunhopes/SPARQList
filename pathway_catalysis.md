# pathway_caltalysis_enzyme_upload

## Parameters
* `pw_id` pathway id
* `control_action` controller action value(activation or inhibition)

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