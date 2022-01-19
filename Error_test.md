# pathway_structure_upload

## Parameters
* `reactionIds`  reaction number array

## `reactionArray`
```javascript
({reactionIds}) => {
  reactionIds = reactionIds.replace(/,/g," ")
  if (reactionIds.match(/[^\s]/)) 
    return reactionIds.split(/\s+/);
  return false;
}
```

## Endpoint
http://path-virtuoso:8890/sparql

## `input_table` 

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX : <http://glycosmos.org/biopax/pathway#>
INSERT DATA
{
    GRAPH <http://localhost:8890/proteinPathwayUpload>
    { 
       :GC-Pathway{{pathway_id}} rdf:type bp:Pathway ;
            bp:displayName "{{pathway_name}}"^^xsd:string ;
            bp:comment "{{pathway_comment}}"^^xsd:string ;
            bp:organism  :GC-Biosource{{pathway_id}};
            rdfs:seeAlso obo:{{ pathway_pw_category_id }};
            skos:altLabel "{{gpathway_pw_category}}"^^xsd:string .
              
     :GC-Biosource{{pathway_id}} rdf:type bp:Biosource ;
            bp:name  "{{pathway_taxon_name}}"^^xsd:string ;
             #bp:xref  glycosmos:GC-UnificationXref{pathway_taxon_id}};
             bp:tissue :GC-TissueVoca{{pathway_id}};
             bp:cellType :GC-CellVocabulary{{pathway_id}} .   
      
       # :GC-UnificationXref{{pathway_taxon_id}} rdf:type bp:UnificationXref;      
                #bp:db "NCBI TAXON"^^xsd:string;
                #bp:id "{{pathway_taxon_id}}"^^xsd:string .
                
     #:GC-TissueVoca-{{pathway_id}} rdf:type bp:TissueVocabulary;
                #bp:xref glycosmos:GC-UnificationXref{{pathway_tissue_id}}.
                     
     #:GC-UnificationXref{{pathway_tissue_id}} rdf:type bp:UnificationXref ;
                #bp:db "Brenda Tissue Ontology"^^xsd:string ;
                #bp:id "{{pathway_tissue_id}}"^^xsd:string .
                
     #:GC-CellVocabulary{{pathway_id}} rdf:type bp:CellVocabulary ;
                #bp:xref glycosmos:GC-UnificationXref{{pathway_cell_id}} .
                
     #:GC-UnificationXref{{pathway_cell_id}} rdf:type bp:UnificationXref ;
               # bp:db "Cell Ontolgy"^^xsd:string ;
               # bp:id  obo:{{pathway_cell_id}}.
                
    # :GC-UnificationXref-{{ pathway_taxon_id}} rdf:type bp:UnificationXref ;
                #bp:db "NCBI taxon ontology"^^xsd:string ;
                #bp:id  ncbitax:{{pathway_taxon_id}} .       
      
     {{#each reactionArray}}
      :GC-Pathway{{pathway_id}} bp:pathwayComponent :GC-BiochemicalReaction{{this}} ;
                bp:pathwayOrder :GC-PathwayStep{{this}} .
     {{/each}}   
    
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