# pathway_controller

## Parameters
* `pw_id` pathway id
* `uniprot_name` uniprot protein name
* `control_complex_name` complex input name
* `control_action`control action direction input

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
	    :{{pw_id}}_GC-Catalysis{{catalysis_node_num}} rdf:type bp:Catalysis ;
            		bp:controlled :{{pw_id}}_GC-BiochemicalReaction{{catalysis_node_num}} ;
            		bp:controller :{{pw_id}}_GC-{{control_node_name}} .
            		##bp:xref :{{pw_id}}_GC-RelationshipXref{{unixref_num_2}}.
      				
      
      	:{{pw_id}}_GC-PathwayStep{{catalysis_node_num}} rdf:type bp:PathwayStep ;
            		bp:stepProcess :{{pw_id}}_GC-Catalysis{{catalysis_node_num}} .
            
       {{#if uniprot_name}}  # if protein
            :{{pw_id}}_GC-{{control_node_name}} rdf:type bp:Protein ;
            		bp:cellularLocation :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} ;
            		bp:name "{{uniprot_name}}"^^xsd:string  ;
            		bp:displayName "{{symbol_name}}"^^xsd:string .
			
         	:{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} rdf:type bp:CellularLocationVocabulary ;
                    bp:term "{{cell_location}}"^^xsd:string; 
                    bp:xref :{{pw_id}}_{{catalysis_node_num}}_GC-UnificationXref{{unixref_num_1}} .
         
         	:{{pw_id}}_{{catalysis_node_num}}_GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificationXref ;
                 	bp:db "GENE ONTOLOGY"^^xsd:string ;
                    bp:id "{{cell_location_id}}"^^xsd:string .
      		
     	{{/if}} 
         
    	{{#if control_complex_name}}   #if complex
          	:{{pw_id}}_GC-{{control_node_name}} rdf:type bp:Complex ;
                    bp:cellularLocation :{{pw_id}}_GC-CellularLocationVocabulary{{control_comp_nodeNum}} .                                               
            
            :{{pw_id}}_GC-CellularLocationVocabulary{{control_comp_nodeNum}} rdf:type bp:CellularLocationVocabulary ;
                    bp:term "{{cell_location}}"^^xsd:string; 
                    bp:xref :{{pw_id}}_{{catalysis_node_num}}_GC-UnificationXref{{unixref_num_1}} .
         
         	:{{pw_id}}_{{catalysis_node_num}}_GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificationXref ;
                 	bp:db "GENE ONTOLOGY"^^xsd:string ;
                    bp:id "{{cell_location_id}}"^^xsd:string .
          
                {{#if complex_goName}}
            	     :{{pw_id}}_GC-{{control_node_name}} bp:displayName "{{complex_goName}}"^^xsd:string .
				
                     :{{pw_id}}_{{catalysis_node_num}}_GC-Stoichiometry{{control_comp_nodeNum}}  rdf:type bp:Stoichiometry ;
                            bp:physicalEntity :{{pw_id}}_GC-{{control_comp_nodeName}} .
                           # bp:stoichiometricCoefficient "1.0"^^xsd:float .
                {{/if}}
              
                {{#if complex_autoCompose_name}} 
             	    :{{pw_id}}_GC-{{control_node_name}} bp:component :{{pw_id}}_GC-{{control_comp_nodeName}} 
            
                {{/if}}
         {{/if}}
         
       {{#if control_action}}
           :{{pw_id}}_GC-Catalysis{{catalysis_node_num}} bp:controlType "{{control_action}}"^^xsd:string .
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