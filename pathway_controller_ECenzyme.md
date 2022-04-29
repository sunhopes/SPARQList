# pathway_controller_ECenzyme

## Parameters
* `control_action` controller action value(activation or inhibition)
* `uniprot_name` if resource is protein
* `control_complex_name` if resource is complex
* `EC_enzyme_name` if resource is protein
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
         :{{pw_id}}_GC-{{catalysis_controller}} rdf:type bp:Protein;
                    bp:cellularLocation :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} ;
            		bp:displayName "{{EC_enzyme_name}}"^^xsd:string ;                                      
      				bp:xref :{{pw_id}}_GC-UnificationXref{{unixref_num_2}} .
    		
         :{{pw_id}}_GC-UnificationXref{{unixref_num_2}} rdf:type bp:UnificationXref ;
            		bp:db "EC enzyme"^^xsd:string ;
           			bp:id "{{EC_enzyme_id}}"^^xsd:string .
     
     	 :{{pw_id}}_GC-BiochemicalReaction{{catalysis_node_num}} bp:eCNumber  "{{EC_enzyme_id}}"^^xsd:string .
       
         :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num} rdf:type bp:CellularLocationVocabulary ;
                    bp:term "{{celllar_location}}"^^xsd:string; 
                    bp:xref :{{pw_id}}_{{catalysis_node_num}}_GC-UnificationXref{{unixref_num_1}} .
         
         :{{pw_id}}_{{catalysis_node_num}}_GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificationXref ;
                 	bp:db "GENE ONTOLOGY"^^xsd:string ;
                    bp:id "{{celllar_location_id}}"^^xsd:string .
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