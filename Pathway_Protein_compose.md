# protein_resource_upload

## Parameters
* `fragment_node_first`  recognition of fragment information 
* `modify_site`  recognition of modify information
* `celllar_location`  protein node reference2 for cellular location
* `uniprot_name`  recognition of modify information
## Endpoint
http://path-virtuoso:8890/sparql

## `input_table` 

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX glytoucan: <http://identifiers.org/glytoucan/>
PREFIX : <http://glycosmos.org/biopax/pathway#>
INSERT DATA
{
    ##GRAPH <http://localhost:8890/proteinPathwayUpload>
    #GRAPH <http://localhost:8890/testSpace>
    GRAPH <http://localhost:8890/testSpace2>
    {    
    ### Protein
    :{{pw_id}}_GC-{{protein_node_name}} rdf:type bp:Protein ;
            bp:cellularLocation :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} ;
            bp:xref :{{pw_id}}_GC-UnificationXref{{unixref_num_1}} ;
            bp:displayName "{{symbol_name}}"^^xsd:string .
      
     ## Xref_first for protein
    {{#if uniprot_name}}
      :{{pw_id}}_GC-{{protein_node_name}}  bp:name "{{uniprot_name}}"^^xsd:string  ;
                bp:xref  :{{pw_id}}_GC-UnificationXref{{unixref_num_1}} .   
      
      :{{pw_id}}_GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificationXref ;
				bp:db "UniProt"^^xsd:string ;
                bp:id "{{ uniprot_id }}"^^xsd:string .
    
    {{/if}}  
      
    ## Xref_second for CellularLocation
	{{#if celllar_location}}  
      :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} rdf:type bp:CellularLocationVocabulary ;
            bp:xref :{{pw_id}}_GC-UnificationXref{{unixref_num_2}} ;
            bp:term "{{celllar_location}}"^^xsd:string . 
     
    :{{pw_id}}_GC-UnificationXref{{unixref_num_2}} rdf:type bp:UnificationXref ;
            bp:db "GENE ONTOLOGY"^^xsd:string ;
            bp:id "{{celllar_location_id}}"^^xsd:string .
      
   {{/if}}
      
   {{#if fragment_node_first}}
     :{{pw_id}}_GC-{{protein_node_name}} bp:feature :{{pw_id}}_GC-FragmentFeature{{protein_node_num}} .
   
      ### FrgmentFeatures(protein size)  
      :{{pw_id}}_GC-FragmentFeature{{protein_node_num}} rdf:type bp:FragmentFeature ;
              bp:featureLocation :{{pw_id}}_GC-SequenceInterval{{protein_node_num}} .

      ### SequenceInterval(for Fragmentation)
      :{{pw_id}}_GC-SequenceInterval{{protein_node_num}} rdf:type bp:SequenceInterval ;
              bp:sequenceIntervalBegin :{{pw_id}}_GC-SequenceSite{{fragment_node_first}} ;
              bp:sequenceIntervalEnd :{{pw_id}}_GC-SequenceSite{{fragment_node_second}} .

      ### SequenceSite(for Fragment start site)
      :{{pw_id}}_GC-SequenceSite{{fragment_node_first}} rdf:type bp:SequenceSite ;
              bp:positionStatus "EQUAL"^^xsd:string ;
              bp:sequencePosition "{{seq_begin}}"^^xsd:string .

      ### SequenceSite (for Fragment end site)
       :{{pw_id}}_GC-SequenceSite{{fragment_node_second}} rdf:type bp:SequenceSite ;
              bp:positionStatus "EQUAL"^^xsd:string 
              bp:sequencePosition "{{seq_end}}"^^xsd:string .
  {{/if}}   
      
  {{#if modify_site}}  
    :{{pw_id}}_GC-{{protein_node_name}}  bp:feature :{{pw_id}}_GC-ModificationFeature{{protein_node_num}}.
    
    ## ModificationFeature (Proein) 
    :{{pw_id}}_GC-ModificationFeature{{protein_node_num}} rdf:type bp:ModificationFeature ;
           bp:featureLocation :{{pw_id}}_GC-SequenceSite{{modify_node_num}} ;
           bp:modificationType :{{pw_id}}_GC-SequenceModificationVocabulary{{protein_node_num}} . 
   
    ## SequenceSite for Modification features (modification site)
    :{{pw_id}}_GC-SequenceSite{{modify_node_num}} rdf:type bp:SequenceSite ;
            bp:positionStatus "EQUAL"^^xsd:string ;
            bp:sequencePosition "{{modify_site}}"^^xsd:string .
    
    ## SequenceModificationVocabulary (modification ontology)   
    :{{pw_id}}_GC-SequenceModificationVocabulary{{protein_node_num}} rdf:type bp:SequenceModificationVocabulary ;
            bp:xref :{{pw_id}}_{{rxn_id}}_GC-UnificationXref{{unixref_num_1}};
            bp:term "{{modify_type}}"^^xsd:string .
    :{{pw_id}}_{{rxn_id}}_GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificatinXref;
            bp:db "MOD"^^xsd:string ;
            bp:id "MOD:"{{modify_id}}^^xsd:string .
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