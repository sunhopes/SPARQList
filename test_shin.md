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
  * `gpathway_bind_backbone` kind of glycan    

## Endpoint
http://path-virtuoso:8890/sparql


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
    GRAPH <http://localhost:8890/upTest>
    {    
     glycosmos:GC-Saccharide-{{reactant_touid}} a bp:SmallMolecule ;
                rdf:type glycordf:Saccharide ;
                bp:xref glycosmos:GC-UnificationXref-{{reactant_touid}} ;
                bp:displayName "{{reactant_name}}"^^xsd:string .
     
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