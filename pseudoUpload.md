# test_Insert_partial_RDF

## Parameters
* `reactant_touid`
	* default: 
* `reactant_name`
	* default: Glucose
* `sugar_id` sugar nucleotide id of PubChem
    * default: 201    
* `sugar_name` sugar nucleotide name in Essential glycobiology
	* default: 
* `enzyme_id` EC enzyme number
	* default: 1234
* `enzyme_name` EC enzyme name    
	* default: Glycan Transferase
* `product_touid` GlyTouCan ID of product glycan
	* default:
* `product_name` glycan name of product  glycan in input
	* default: Mannose  
* `cell_locate_id` GO cellular component ontology Id
    * default:
* `cell_locate` GO cellular component ontology name
    * default: cytosol
* `reaction_id` instance id of BiochemicalReaction Class
	* default: 201
* `reactionIds`
	* default: 201,202,203    
* `gpathway_id`
	* default: 21
* `gpathway_name` gpathway title    
    * default: Test 
* `gpathway_comment` gpathway description
	* default: gpathway regist test
* `gpathway_tissue_id` tissue id ontology for biobanking
	* default: 
* `gpathway_tissue_name` tissue name in ontology for biobanking
	* default: Blood
* `gpathway_cell_name` cell type ontology name
	* default: 
* `gpathway_cell_id` cell type ontology id    
    * default: 
* `gpathway_taxon_name` NCBI taxon common name
	* default:
* `gpathway_taxon_id` NCBI taxon id
	* default: 9606
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
PREFIX gno: <http://purl.obolibrary.org/obo/GNO:>
PREFIX glycosmos: <http://glycosmos.org/biopax/1/562#>
PREFIX glytoucan: <http://identifiers.org/glytoucan/>
PREFIX uniprot: <http://identifiers.org/uniprot/>
PREFIX pubchem: <http://pubchem.ncbi.nlm.nih.gov/compound/>
PREFIX ncbitax: <http://purl.bioontology.org/ontology/NCBITAXON/>

INSERT DATA
{
    GRAPH <http://localhost:8890/upTest>
    {
   	 glycosmos:GC-Saccharide-{{reactant_touid}} a bp:SmallMolecule;
                rdf:type glycordf:Saccharide;	
                bp:displayName {{reactant_name}}^^xsd:string ;
                bp:xref glycosmos:GC-UnificationXref-{{reactant_touid}} .
     
     glycosmos:GC-UnificationXref-{{reactant_touid}} a bp:UnificationXref;
                bp:db "GlyTouCan"^^xsd:string;
                bp:id  glytoucan:{{reactant_touid}}.
                
     glycosmos:GC-Saccharide-{{product_touid}} a bp:SmallMolecule;
                rdf:type glycordf:Saccharide;
                bp:displayName {{product_name}}^^xsd:string ;
                bp:xref glycosmos:GC-UnificationXref-{{product_touid}} ;
                bp:cellularLocation glycosmos:GC-CellularLocation-{{cell_locate_id}}.

     glycosmos:GC-UnificationXref-{{product_touid}} a bp:UnificationXref;
                bp:db "GlyTouCan"^^xsd:string;
                bp:id glytoucan:{{product_touid}}. 
                
     glycosmos:GC-SugarNucleoside-{{reaction_id}} a obo:CHEBI_59737;
                rdf:type bp:SmallMolecule;
                bp:displayName {{sugar_name}}^^xsd:string;
                bp:xref glycosmos:GC-UnificationXref-{{sugar_id}} .           
     
     glycosmos:GC-UnificationXref-{{sugar_id}} a bp:UnificationXref;
                bp:db "PubChem"^^xsd:string;
                bp:id  pubchem:{{sugar_id}}.     
     
     glycosmos:GC-UnificationXref-{{enzyme_id}} a bp:UnificationXref;
                bp:db "ECenzyme"^^xsd:string;
                bp:id glycosmos:{{enzyme_id}}.
                
     glycosmos:GC-Protein-{{reaction_id}} a bp:Protein ; 
                bp:displayName {{enzyme_name}}^^xsd:string ;
                bp:xref glycosmos:GC-UnificationXref-{{enzyme_id}} .
     
     glycosmos:GC-CellularLocation-{{reaction_id}} a bp:CellularLocationVocabulary;
                bp:term  {{cell_locate}}^^xsd:string ;
                bp:xref  glycosmos:GC-UnificationXref-{{cell_locate_id}} .           

     glycosmos:GC-UnificationXref-{{cell_locate_id}} a bp:UnificationXref;             
                bp:db "GO ontology"^^xsd:string;
                bp:id obo:{{cell_locate_id}}.

     glycosmos:GC-RXN-{{reaction_id}} a bp:BiochemicalReaction ;
                bp:left  glycosmos:GC-Saccharide-{{reactant_touid}};
                bp:left  glycosmos:GC-SugarNucleoside-{{reaction_id}};
                bp:right  glycosmos:GC-Saccharide-{{product_touid}} .      

     glycosmos:GC-Catalysis-{{reaction_id}} a bp:Catalysis ;
                bp:controlType  "ACTIVATION"^^xsd:string ;
                bp:controlled  glycosmos:GC-RXN-{{reaction_id}} ;
                bp:controller  glycosmos:GC-Protein-{{reaction_id}} .           
     
     glycosmos:GC-TissueVocaburaly-{{gpathway_id}} a bp:TissueVocabulary ;
                bp:xref glycosmos:GC-UnificationXref-{{gpathway_tissue_id}} ;
                bp:displayName {{gpathway_tissue_name}}^^xsd:string .

     glycosmos:GC-UnificationXref-{{gpathway_tissue_id}} a bp:UnificationXref ;
                bp:db "Ontlogy of Biobank"^^xsd:string ;
                bp:id  obo:{{gpathway_tissue_id}} . 
                
     glycosmos:GC-CellVocabulary-{{gpathway_id}} a bp:CellVocabulary ;
                bp:xref glycosmos:GC-UnificationXref-{{gpathway_cell_id}} .           
                
     glycosmos:GC-UnificationXref-{{gpathway_cell_id}} a bp:UnificationXref ;
                bp:db "Cell Ontolgy"^^xsd:string ;
                bp:id  obo:{{gpathway_cell_id}};
                bp:displayName  {{gpathway_cell_name}}^^xsd:string .           
             
     glycosmos:GC-UnificationXref-{{ gpathway_taxon_id}} a bp:UnificationXref ;
                bp:db "NCBI taxon ontology"^^xsd:string ;
                bp:id  ncbitax:{{gpathway_taxon_id}};
                bp:displayName {{gpathway_taxon_name}}^^xsd:string . 
                
  {{#each reactionArrayNext}}
     glycosmos:GC-PathwayStep-{{this.val}} a bp:PathwayStep;
	    		bp:nextStep glycosmos:GC-PathwayStep-{{this.next}};
	    		bp:stepConversion glycosmos:GC-RXN-{{this.val}} ;
	    		bp:stepDirection "LEFT-TO-RIGHT"^^xsd:string ;
	    		bp:stepProcess glycosmos:GC-Catalysis-{{this.val}} .
     glycosmos:GC-Pathway-{{this.id}} bp:pathwayComponent glycosmos:GC-RXN-{{this.val}};
                bp:pathwayComponent glycosmos:GC-Catalysis-{{this.val}};
                bp:pathwayOrder glycosmos:GC-PathwayStep-{{this.val}} .
   {{/each}}
    glycosmos:GC-Pathway-{{gpathway_id}} a bp:Pathway ;
                bp:displayName {{gpathway_name}}^^xsd:string ;
                bp:comment {{gpathway_comment}}^^xsd:string ;
                bp:organism  glycosmos:GC-Biosource-{{gpathway_id}}.
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