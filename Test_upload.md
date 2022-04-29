# test_protein_upload

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
PREFIX cco: <http://www.biocyc.org/owl/ontologies/ocelot/cco/#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX glytoucan: <http://identifiers.org/glytoucan/>
PREFIX uniprot: <http://identifiers.org/uniprot/>
PREFIX pubchem: <http://pubchem.ncbi.nlm.nih.gov/compound/>
PREFIX ncbitax: <http://purl.bioontology.org/ontology/NCBITAXON/>
PREFIX sio: <http://semanticscience.org/resource/>
<<<<<<< HEAD
PREFIX : <http://glycosmos.org/biopax/pathway/>
=======
PREFIX : <http://glycosmos.org/biopax/pathway#>
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
INSERT DATA
{
    GRAPH <http://localhost:8890/testSpace>
    {    
    ### Protein
<<<<<<< HEAD
    :{{pw_id}}_GC-{{protein_node_name}} rdf:type bp:Protein ;
            bp:cellularLocation :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} ;
            #bp:xref :{{pw_id}}_GC-UnificationXref{{unixref_num_1}} ;
=======
    :GC-{{protein_node_name}} rdf:type bp:Protein ;
            bp:cellularLocation :GC-CellularLocationVocabulary{{protein_node_num}} ;
            #bp:xref :GC-UnificationXref{{unixref_num_1}} ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
            bp:name "{{uniprot_name}}"^^xsd:string  ;
            bp:displayName "{{symbol_name}}"^^xsd:string .
      
     ### ProteinXref
    {{#if uniprot_name}}
<<<<<<< HEAD
      :{{pw_id}}_GC-{{protein_node_name}} bp:xref :GC-UnificationXref{{unixref_num_1}} .   
      :{{pw_id}}_GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificationXref ;
=======
      :GC-{{protein_node_name}} bp:xref :GC-UnificationXref{{unixref_num_1}} .   
      :GC-UnificationXref{{unixref_num_1}} rdf:type bp:UnificationXref ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
            bp:db "UniProt"^^xsd:string ;
            bp:id "{{uniprot_id}}"^^xsd:string . 
    {{/if}}  
      
    ### CellularLocation
    {{#if celllar_location}}  
<<<<<<< HEAD
    :{{pw_id}}_GC-CellularLocationVocabulary{{protein_node_num}} rdf:type bp:CellularLocationVocabulary ;
            bp:xref :{{pw_id}}_GC-UnificationXref{{unixref_num_2}} ;
            bp:term "{{celllar_location}}"^^xsd:string . 
      
    ### CellVocaXref  
    :{{pw_id}}_GC-UnificationXref{{unixref_num_2}} rdf:type bp:UnificationXref ;
=======
    :GC-CellularLocationVocabulary{{protein_node_num}} rdf:type bp:CellularLocationVocabulary ;
            bp:xref :GC-UnificationXref{{unixref_num_2}} ;
            bp:term "{{celllar_location}}"^^xsd:string . 
      
    ### CellVocaXref  
    :GC-UnificationXref{{unixref_num_2}} rdf:type bp:UnificationXref ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
            bp:db "GENE ONTOLOGY"^^xsd:string ;
            bp:id "{{celllar_location_id}}"^^xsd:string .
      
    {{/if}}
      
   {{#if fragment_node_first}}
<<<<<<< HEAD
     :{{pw_id}}_GC-{{protein_node_name}} bp:feature :{{pw_id}}_GC-FragmentFeature{{protein_node_num}} .
   
      ### FrgmentFeatures(protein size)  
      :{{pw_id}}_GC-FragmentFeature{{protein_node_num}} rdf:type bp:FragmentFeature ;
              bp:featureLocation :{{pw_id}}_GC-SequenceInterval{{protein_node_num}} .

      ### SequenceInterval(for Fragmentation)
      :{{pw_id}}_GC-SequenceInterval{{protein_node_num}} rdf:type bp:SequenceInterval ;
              bp:sequenceIntervalBegin :{{pw_id}}_GC-SequenceSite{{fragment_node_first}} ;
              bp:sequenceIntervalEnd :{{pw_id}}_GC-SequenceSite{{fragment_node_second}} .

      ### SequenceSite(for Fragment)
      :{{pw_id}}_GC-SequenceSite{{fragment_node_first}} rdf:type bp:SequenceSite ;
=======
     :GC-{{protein_node_name}} bp:feature :GC-FragmentFeature{{protein_node_num}} .
   
      ### FrgmentFeatures(protein size)  
      :GC-FragmentFeature{{protein_node_num}} rdf:type bp:FragmentFeature ;
              bp:featureLocation :GC-SequenceInterval{{protein_node_num}} .

      ### SequenceInterval(for Fragmentation)
      :GC-SequenceInterval{{protein_node_num}} rdf:type bp:SequenceInterval ;
              bp:sequenceIntervalBegin :GC-SequenceSite{{fragment_node_first}} ;
              bp:sequenceIntervalEnd :GC-SequenceSite{{fragment_node_second}} .

      ### SequenceSite(for Fragment)
      :GC-SequenceSite{{fragment_node_first}} rdf:type bp:SequenceSite ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
              bp:positionStatus "EQUAL"^^xsd:string ;
              bp:sequencePosition "{{seq_begin}}"^^xsd:integer .

      ### SequenceSite (for Fragment)
<<<<<<< HEAD
       :{{pw_id}}_GC-SequenceSite{{fragment_node_second}} rdf:type bp:SequenceSite ;
=======
       :GC-SequenceSite{{fragment_node_second}} rdf:type bp:SequenceSite ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
              bp:positionStatus "EQUAL"^^xsd:string ;
            bp:sequencePosition "{{seq_end}}"^^xsd:integer .
  {{/if}}   
      
  {{#if modify_site}}  
<<<<<<< HEAD
    :{{pw_id}}_GC-{{protein_node_name}}  bp:feature :{{pw_id}}_GC-ModificationFeature{{protein_node_num}}.
    ### ModificationFeature (Proein) 
    :{{pw_id}}_GC-ModificationFeature{{protein_node_num}} rdf:type bp:ModificationFeature ;
           bp:featureLocation :{{pw_id}}_GC-SequenceSite{{modify_node_num}} ;
           bp:modificationType :{{pw_id}}_GC-SequenceModificationVocabulary{{protein_node_num}} . 
   
    ### SequenceSite for Modification features (modification site)
    :{{pw_id}}_GC-SequenceSite{{modify_node_num}} rdf:type bp:SequenceSite ;
=======
    :GC-{{protein_node_name}}  bp:feature :GC-ModificationFeature{{protein_node_num}}.
    ### ModificationFeature (Proein) 
    :GC-ModificationFeature{{protein_node_num}} rdf:type bp:ModificationFeature ;
           bp:featureLocation :GC-SequenceSite{{modify_node_num}} ;
           bp:modificationType :GC-SequenceModificationVocabulary{{protein_node_num}} . 
   
    ### SequenceSite for Modification features (modification site)
    :GC-SequenceSite{{modify_node_num}} rdf:type bp:SequenceSite ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
            bp:positionStatus "EQUAL"^^xsd:string ;
            bp:sequencePosition "{{modify_site}}"^^xsd:integer .
    
    ### SequenceModificationVocabulary (modification ontology)   
<<<<<<< HEAD
    :{{pw_id}}_GC-SequenceModificationVocabulary{{protein_node_num}} rdf:type bp:SequenceModificationVocabulary ;
=======
    :GC-SequenceModificationVocabulary{{protein_node_num}} rdf:type bp:SequenceModificationVocabulary ;
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
            bp:term "{{modify_type}}"^^xsd:string .
           
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