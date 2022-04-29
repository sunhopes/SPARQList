# pathway_product_Autocomplex_upload

## Parameters
* `pw_id` pathway id
* `rxn_id` reaction id
* `complex_node_num` complex node number in product
* `componentsNodes` reactant node name for component
* `complex_node_name` complex node name
## `compIdPlus`
- complex number
```javascript
({complex_node_num}) => {
    int_num = Number(complex_node_num);
    return int_num +=1;
}
``` 
## `componentArray`
- each for stoichiometry triples
```javascript
({componentsNodes, complex_node_name, rxn_id, pw_id}) => {
  compArray = componentsNodes.replace(/,/g," ").split(/\s+/);
  var component_array = [];
  for (let i=0; i< compArray.length; i++){
    console.log(compArray[i].match(/\d+$/g))
    component_array.push({ 'pwId': pw_id, 'rxnId': rxn_id, 'comp_node_name': compArray[i],'comp_node_num': compArray[i].match(/\d+$/g),'complex_name': complex_node_name })
  }
    return component_array;
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
    GRAPH <http://localhost:8890/testSpace2>
    #GRAPH <http://localhost:8890/testSpace>
  ##GRAPH <http://localhost:8890/proteinPathwayUpload>
    {
     
	:{{pw_id}}_GC-{{complex_node_name}} rdf:type bp:Complex ;
            ##bp:comment "GlyCosmos DB_ID: {{pw_id}}_{{rxn_id}}_{{complex_node_num}}"^^xsd:string ;
            #bp:cellularLocation :{{pw_id}}_{{rxn_id}}_CellularLocationVocabulary{{complex_node_num}} ;
            bp:displayName "{{complex_display_name}}"^^xsd:string .
      
     #:{{pw_id}}_{{rxn_id}}_CellularLocationVocabulary{{complex_node_num}} rdf:type bp:CellularLocationVocabulary ;
           # bp:xref :{{pw_id}}_{{rxn_id}}UnificationXref{{complex_node_num}} ;
           # bp:term "{{complex_cell_loci}}"^^xsd:string .
      
      #:{{pw_id}}_{{rxn_id}}UnificationXref{{complex_node_num}} rdf:type bp:UnificationXref ;
                 # bp:db "GENE ONTOLOGY"^^xsd:string ;
                 # bp:id "{{complex_cell_loci_id}}"^^xsd:string .

      
      
     {{#each componentArray}}
       :{{pwId}}_GC-{{complex_name}}  bp:component :{{pwId}}_GC-{{this.comp_node_name}}.
       :{{pwId}}_GC-{{complex_name}}  bp:componentStoichiometry :{{pwId}}_{{rxnId}}_GC-Stoichiometry{{this.comp_node_num}}.
       :{{pwId}}_{{rxnId}}_GC-Stoichiometry{{this.comp_node_num}} rdf:type bp:Stoichiometry ;
               bp:physicalEntity :{{pwId}}_GC-{{this.comp_node_name}} ;
               bp:stoichiometricCoefficient "1.0"^^xsd:float .
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