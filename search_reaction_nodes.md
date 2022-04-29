# reaction node name in same pathway(GoJS)

## Parameters
* `pathway_id` get pathway_id from rails

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>

SELECT DISTINCT  ?reaction (GROUP_CONCAT(DISTINCT ?reaction_name ; separator = ",") AS ?reaction_names)  (GROUP_CONCAT(DISTINCT ?reactant_names ; separator = ",") AS ?reactant_names_arry)  (GROUP_CONCAT(DISTINCT ?product_names ; separator = ",") AS ?product_name_arry) ?controller_name
FROM <http://localhost:8890/testSpace>
WHERE{ 
    <http://glycosmos.org/biopax/pathway#GC-Pathway{{pathway_id}}> bp:pathwayOrder ?pathwaystep .
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