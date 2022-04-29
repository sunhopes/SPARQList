# Detail information of instances in proteinpathway

## Parameters
* `pw_id` get pathway_id 

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

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
PREFIX pwid: <http://glycosmos.org/biopax/pathway#>

SELECT DISTINCT  ?reaction (GROUP_CONCAT(DISTINCT ?reaction_name ; separator = ",") AS ?reaction_names)  (GROUP_CONCAT(DISTINCT ?reactant_names ; separator = ",") AS ?reactant_names_arry)  (GROUP_CONCAT(DISTINCT ?product_names ; separator = ",") AS ?product_name_arry) ?controller_name
FROM <http://localhost:8890/testSpace>
WHERE{ 
    pwid:GC-Pathway{{pw_id}} bp:pathwayOrder ?pathwaystep .
    ?pathwaystep bp:stepProcess ?reaction .
    ?reaction rdf:type bp:BiochemicalReaction .
    ?reaction bp:left ?reactant .
    ?reaction bp:right ?product .
    ?reactant bp:displayName ?reactant_name .
    ?product bp:displayName ?product_name .

    BIND (replace(str(?reaction), 'http://glycosmos.org/biopax/pathway#', '') AS ?reaction_name)
    BIND (replace(str(?reactant_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_names)
    BIND (replace(str(?product_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?product_names)
    
    OPTIONAL {
    ?pathwaystep bp:stepProcess ?catalysis .
    ?catalysis rdf:type bp:Catalysis . 
    ?catalysis bp:controller ?controller .
    ?controller bp:displayName ?controller_name .
    }

}
ORDER BY ?reaction

```

## Output
```javascript

({
  json({list}) {
    return list.results.bindings.map((row) => {
      let reaction_num_array = row.reaction_names.value.split(',')
      let reactants_array = row.reactant_names_arry.value.split(',')
      let products_array = row.product_name_arry.value.split(',')
      if (row.controller_name) { 
        controller_name = row.controller_name.value ;
      } else { 
        controller_name = "";
      }
      return {
        "reaction_id": reaction_num_array,
        "reactant_array": reactants_array,
        "controller": controller_name,
        "product_array": products_array
      } 
    });  
  }
})

```