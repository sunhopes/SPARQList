# Input_test

## Parameters
* `reaction_id` instance id of BiochemicalReaction Class 
* `reactant_name` glycan name of reactant glycan in input
* `reactant_touid` glyTouCan id
* `product_name` glycan name of product  glycan in input
* `product_touid` GlyTouCan ID of product glycan
* `sugar_id` sugar nucleotide id of PubChem
* `sugar_name` sugar nucleotide name in Essential glycobiology
* `enzyme_id` EC enzyme number
* `enzyme_name` EC enzyme name
* `cell_locate_id` GO cellular component ontology Id
* `cell_locate` GO cellular component ontology name
* `reactionIds` Array of reaction numbers
* `gpathway_id` gpathway Id
* `gpathway_name` gpathway title 
* `gpathway_comment` gpathway description
* `gpathway_tissue_id` tissue id ontology for biobanking
* `gpathway_tissue_name` tissue name in ontology for biobanking
* `gpathway_cell_name` cell type ontology name
* `gpathway_cell_id` cell type ontology id
* `gpathway_taxon_name` NCBI taxon common name
* `gpathway_taxon_id` NCBI taxon id
* `gpathway_pw_category_id` pathway ontology id
* `gpathway_pw_category` pathway ontology name   

## Endpoint
http://path-virtuoso:8890/sparql

## `reactionArray`
```javascript
({reactionIds}) => {
    reactionIds = reactionIds.replace(/,/g," ")
    if (reactionIds.match(/[^\s]/)) return reactionIds.split(/\s+/);
    return false;
  }
```
## `reactionArrayNext`
```javascript
({reactionArray,gpathway_id}) => {
	var newObjList = [];
    for(let i=0; i< reactionArray.length-1; i++){
    	newObjList.push(
        	{ val:reactionArray[i],
              next:reactionArray[i+1],
              id:gpathway_id} )
    }
    return newObjList;
 }
```

## `input_table` Query adjacent prefectures

```sparql
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX cco: <http://www.biocyc.org/owl/ontologies/ocelot/cco/#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX glycordf: <http://purl.jp/bio/12/glyco/glycan#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX gno: <http://purl.obolibrary.org/obo/GNO:>
PREFIX glycosmos: <http://glycosmos.org/biopax/1/562#>
PREFIX glytoucan: <http://identifiers.org/glytoucan/>
PREFIX uniprot: <http://identifiers.org/uniprot/>
PREFIX pubchem: <http://pubchem.ncbi.nlm.nih.gov/compound/>
PREFIX ncbitax: <http://purl.bioontology.org/ontology/NCBITAXON/>

INSERT DATA
{
    GRAPH <http://localhost:8890/repo_test1> 
    {
     glycosmos:GC-Saccharide-{{reactant_touid}} a bp:SmallMolecule ;
                rdf:type glycordf:Saccharide ;
                bp:xref glycosmos:GC-UnificationXref-{{reactant_touid}} ;
                bp:displayName "{{reactant_name}}"^^xsd:string .
      
     glycosmos:GC-UnificationXref-{{reactant_touid}} a bp:UnificationXref;
                bp:db "GlyTouCan"^^xsd:string;
                bp:id  glytoucan:{{reactant_touid}}.
                
     glycosmos:GC-Saccharide-{{product_touid}} a bp:SmallMolecule;
                rdf:type glycordf:Saccharide;
                bp:displayName "{{product_name}}"^^xsd:string ;
                bp:xref glycosmos:GC-UnificationXref-{{product_touid}} ;
                bp:cellularLocation glycosmos:GC-CellularLocation-{{reaction_id}}.

     glycosmos:GC-UnificationXref-{{product_touid}} a bp:UnificationXref;
                bp:db "GlyTouCan"^^xsd:string;
                bp:id glytoucan:{{product_touid}}.                     
                
     glycosmos:GC-CellularLocation-{{reaction_id}} a bp:CellularLocationVocabulary;
                bp:term  "{{cell_locate}}"^^xsd:string ;
                bp:xref  glycosmos:GC-UnificationXref-{{cell_locate_id}} .
      
     glycosmos:GC-CellularLocation-{{reaction_id}} a bp:CellularLocationVocabulary;
                bp:term  "{{cell_locate}}"^^xsd:string ;
                bp:xref  glycosmos:GC-UnificationXref-{{cell_locate_id}} .
                
     glycosmos:GC-UnificationXref-{{cell_locate_id}} a bp:UnificationXref;             
                bp:db "GO ontology"^^xsd:string;
                bp:id obo:{{cell_locate_id}}.           
                
     glycosmos:GC-SugarNucleoside-{{reaction_id}} a obo:CHEBI_59737;
                rdf:type bp:SmallMolecule;
                bp:displayName "{{sugar_name}}"^^xsd:string;
                bp:xref glycosmos:GC-UnificationXref-{{sugar_id}} . 
                
     glycosmos:GC-UnificationXref-{{sugar_id}} a bp:UnificationXref;
                bp:db "PubChem"^^xsd:string ;
                bp:id  pubchem:{{sugar_id}}. 
                
     glycosmos:GC-UnificationXref-{{enzyme_id}} a bp:UnificationXref;
                bp:db "Enzyme Commission number"^^xsd:string;
                bp:id "{{enzyme_id}}"^^xsd:string .
                
     glycosmos:GC-Protein-{{reaction_id}} a bp:Protein ; 
                bp:displayName "{{enzyme_name}}"^^xsd:string ;
                bp:xref glycosmos:GC-UnificationXref-{{enzyme_id}} .           
 
     glycosmos:GC-Catalysis-{{reaction_id}} a bp:Catalysis ;
                bp:controlType  "ACTIVATION"^^xsd:string ;
                bp:controlled  glycosmos:GC-RXN-{{reaction_id}} ;
                bp:controller  glycosmos:GC-Protein-{{reaction_id}} .
                
     glycosmos:GC-RXN-{{reaction_id}} a bp:BiochemicalReaction ;
                bp:left  glycosmos:GC-Saccharide-{{reactant_touid}};
                bp:left  glycosmos:GC-SugarNucleoside-{{reaction_id}};
                bp:right  glycosmos:GC-Saccharide-{{product_touid}} . 
      
      glycosmos:GC-Pathway-{{gpathway_id}} a bp:Pathway ;
                bp:displayName "{{gpathway_name}}"^^xsd:string ;
                bp:comment "{{gpathway_comment}}"^^xsd:string ;
                bp:organism  glycosmos:GC-Biosource-{{gpathway_id}};
                rdfs:seeAlso obo:{{ gpathway_pw_category_id }};
                skos:altLabel "{{gpathway_pw_category}}"^^xsd:string .
                
     glycosmos:GC-Biosource-{{gpathway_id}} a bp:Biosource;
                bp:name  "{{gpathway_taxon_name}}"^^xsd:string;
                #bp:xref  glycosmos:GC-UnificationXref-{{gpathway_taxon_id}};
                bp:tissue glycosmos:GC-TissueVoca-{{gpathway_id}};
                bp:cellType glycosmos:GC-CellVocabulary-{{gpathway_id}} .
       
    # glycosmos:GC-UnificationXref-{{gpathway_taxon_id}} a bp:UnificationXref;      
                #bp:db "NCBI TAXON"^^xsd:string;
                #bp:id "{{gpathway_taxon_id}}"^^xsd:string .
                
     glycosmos:GC-TissueVoca-{{gpathway_id}} a bp:TissueVocabulary;
                bp:xref glycosmos:GC-UnificationXref-{{gpathway_tissue_id}}.
                     
     glycosmos:GC-UnificationXref-{{gpathway_tissue_id}} a bp:UnificationXref ;
                bp:db "Brenda Tissue Ontology"^^xsd:string;
                bp:id "{{gpathway_tissue_id}}"^^xsd:string .
                
     glycosmos:GC-CellVocabulary-{{gpathway_id}} a bp:CellVocabulary ;
                bp:xref glycosmos:GC-UnificationXref-{{gpathway_cell_id}} .
                
     glycosmos:GC-UnificationXref-{{gpathway_cell_id}} a bp:UnificationXref ;
                bp:db "Cell Ontolgy"^^xsd:string ;
                bp:id  obo:{{gpathway_cell_id}}.
                
    # glycosmos:GC-UnificationXref-{{ gpathway_taxon_id}} a bp:UnificationXref ;
                #bp:db "NCBI taxon ontology"^^xsd:string ;
                #bp:id  ncbitax:{{gpathway_taxon_id}} .               
                 
     {{#each reactionArrayNext}}
        glycosmos:GC-PathwayStep-{{this.val}} a bp:BiochemicalPathwayStep;
	    		bp:nextStep glycosmos:GC-PathwayStep-{{this.next}};
	    		bp:stepConversion glycosmos:GC-RXN-{{this.val}} ;
	    		bp:stepDirection "LEFT-TO-RIGHT"^^xsd:string ;
	    		bp:stepProcess glycosmos:GC-Catalysis-{{this.val}} .
        glycosmos:GC-Pathway-{{this.id}} bp:pathwayComponent glycosmos:GC-RXN-{{this.val}};
                bp:pathwayComponent glycosmos:GC-Catalysis-{{this.val}};
                bp:pathwayOrder glycosmos:GC-PathwayStep-{{this.val}} .
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