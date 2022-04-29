# protein_pathway_upload

## Parameters
* `reaction_id` instance id of BiochemicalReaction Class 

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
({reactionArray}) => {
	var newObjList = [];
    for(let i=0; i< reactionArray.length-1; i++){
    	newObjList.push(
        	{ val:reactionArray[i],
              next:reactionArray[i+1]} )
    }
    return newObjList;
 }
```

## `pwCompArray`
```javascript
({reactionArray,gpathway_id}) => {
	var compList = [];
    for(let i=0; i< reactionArray.length; i++){
    	 compList.push(
        	{ value: reactionArray[i],
              pwid: gpathway_id} )
    }
    return compList;
 }
```

## `input_table` Query adjacent prefectures

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
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
PREFIX sio: <http://semanticscience.org/resource/>

INSERT DATA
{
    GRAPH <http://localhost:8890/proteinPathwayUpload>
    {    
      
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