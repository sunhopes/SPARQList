# pathway_reaction_upload

## Parameters
* `reactantNodes` reactant array 
* `productNodes` product array 
* `rxn_id` rxn id
<<<<<<< HEAD
* `pw_id` pathway id
=======
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
## `reactantArray`
```javascript
({reactantNodes}) => {
  reactantNodes = reactantNodes.replace(/,/g," ")
  if (reactantNodes.match(/[^\s]/)) 
    return reactantNodes.split(/\s+/);
  return false;
}
``` 
## `reactantArray_id`
```javascript
<<<<<<< HEAD
({reactantArray, rxn_id, pw_id}) => {
  var reactant_array = [];
  for (let i=0; i< reactantArray.length; i++){
    	reactant_array.push({ 'pwid': pw_id, 'val': reactantArray[i], 'id': rxn_id})
=======
({reactantArray, rxn_id}) => {
  var reactant_array = [];
  for (let i=0; i< reactantArray.length; i++){
    	reactant_array.push({'val': reactantArray[i], 'id': rxn_id})
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
  }
    return reactant_array;
  }
```
## `productArray`
```javascript
({productNodes}) => {
  productNodes = productNodes.replace(/,/g," ")
  if (productNodes.match(/[^\s]/)) 
    return productNodes.split(/\s+/);
  return false;
}
```
## `productArray_id`
```javascript
<<<<<<< HEAD
({productArray, rxn_id, pw_id}) => {
  var product_array = [];
  for (let i=0; i< productArray.length; i++){
    	product_array.push(
        	{ 'pwid': pw_id, 'val': productArray[i], 'id': rxn_id} )
=======
({productArray, rxn_id}) => {
  var product_array = [];
  for (let i=0; i< productArray.length; i++){
    	product_array.push(
        	{ val: productArray[i], id: rxn_id} )
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
    }
    return product_array;
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
    ###GRAPH <http://localhost:8890/proteinPathwayUpload>
<<<<<<< HEAD
    #GRAPH <http://localhost:8890/testSpace>
    GRAPH<http://localhost:8890/testSpace2>
    {
      :{{pw_id}}_GC-BiochemicalReaction{{rxn_id}} rdf:type bp:BiochemicalReaction ;
             bp:conversionDirection "LEFT-TO-RIGHT"^^xsd:string .
             ###bp:displayName "Get from users"^^xsd:string .
      
    ### reactant node
      {{#each reactantArray_id}}
        :{{pwid}}_GC-BiochemicalReaction{{this.id}} bp:left :{{pwid}}_GC-{{this.val}} .
      {{/each}}
    #bp:participantStoichiometry :Stoichiometry33 , :Stoichiometry34 ;
        
    ### product node  
      {{#each productArray_id}}
        :{{pwid}}_GC-BiochemicalReaction{{this.id}} bp:right :{{pwid}}_GC-{{this.val}} .
      {{/each}}
           
      :{{pw_id}}_GC-PathwayStep{{rxn_id}} rdf:type bp:PathwayStep ;
             bp:stepProcess :{{pw_id}}_GC-BiochemicalReaction{{rxn_id}} .
=======
    GRAPH <http://localhost:8890/testSpace>      
    {
      :GC-BiochemicalReaction{{rxn_id}} rdf:type bp:BiochemicalReaction ;
             bp:conversionDirection "LEFT-TO-RIGHT"^^xsd:string .
             ###bp:displayName "Get from users"^^xsd:string .
    ### reactant node
      {{#each reactantArray_id}}
        :GC-BiochemicalReaction{{this.id}} bp:left :GC-{{this.val}} .
      {{/each}}
    
      ###bp:participantStoichiometry :Stoichiometry33 , :Stoichiometry34 ;
    ### product node  
      {{#each productArray_id}}
        :GC-BiochemicalReaction{{this.id}} bp:right :GC-{{this.val}} .
      {{/each}}
           
      :GC-PathwayStep{{rxn_id}} rdf:type bp:PathwayStep ;
             bp:stepProcess :GC-BiochemicalReaction{{rxn_id}} .
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
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