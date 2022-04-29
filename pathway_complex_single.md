# pathway_complex_single

## Parameters
* `pw_id` pathway id
* `rxn_id` reaction id

## `compIdPlus`
- complex number
```javascript
({complex_node_num}) => {
    int_num = Number(complex_node_num);
    return int_num +=1;
}
``` 

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
  #GRAPH <http://localhost:8890/testSpace>
  GRAPH <http://localhost:8890/testSpace2>
  ##GRAPH <http://localhost:8890/proteinPathwayUpload>
    {
     
	 :{{pw_id}}_GC-{{complex_node_name}} rdf:type bp:Complex ;
            ##bp:comment "GlyCosmos DB_ID: {{pw_id}}_{{rxn_id}}_{{complex_node_num}}"^^xsd:string ;
            bp:cellularLocation :{{pw_id}}_{{rxn_id}}_CellularLocationVocabulary{{complex_node_num}} ;
            bp:name "{{complex_onto_name}}"^^xsd:string ;
            bp:displayName "{{complex_display_name}}"^^xsd:string .
      
     #:{{pw_id}}_{{rxn_id}}_CellularLocationVocabulary{{complex_node_num}} rdf:type bp:CellularLocationVocabulary ;
            #bp:xref :{{pw_id}}_{{rxn_id}}UnificationXref{{complex_node_num}} ;
            #bp:term "{{cellular_location}}"^^xsd:string .
      
     #:{{pw_id}}_{{rxn_id}}UnificationXref{{complex_node_num}} rdf:type bp:UnificationXref ;
            #bp:db "GENE ONTOLOGY"^^xsd:string ;
            #bp:id "{{celllar_location_id}}"^^xsd:string .
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