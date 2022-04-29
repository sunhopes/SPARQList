# Pathway visualization(Cytoscape)

## Parameters
* `pathway_id` get pathway_id from rails

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>

SELECT DISTINCT  (GROUP_CONCAT(DISTINCT ?reaction_name ; separator = ",") AS ?reaction_names) (GROUP_CONCAT(DISTINCT ?reactant_node ; separator = ",") AS ?reactant_nodes) (GROUP_CONCAT(DISTINCT ?reactant_names ; separator = ",") AS ?reactant_names_arry) (GROUP_CONCAT(DISTINCT ?product_node ; separator = ",") AS ?product_nodes) (GROUP_CONCAT(DISTINCT ?product_names ; separator = ",") AS ?product_name_arry) ?cntrol_node (GROUP_CONCAT(DISTINCT ?comp_node ; separator = ",") AS ?comp_nodes) (GROUP_CONCAT(DISTINCT ?comp_node_name; separator = ",") AS ?comp_names)  #?comp_name (GROUP_CONCAT(DISTINCT ?prod_node_name ; separator = ",") AS ?prod_comp_nodes)
FROM <http://localhost:8890/testSpace2>
WHERE{ 
    <http://glycosmos.org/biopax/pathway#GC-Pathway164> bp:pathwayOrder ?pathwaystep .
    #?s bp:pathwayOrder ?pathwaystep .
    ?pathwaystep bp:stepProcess ?reaction .
    ?reaction rdf:type bp:BiochemicalReaction .
    ?reaction bp:left ?reactant .
    ?reaction bp:right ?product .
    ?reactant bp:displayName ?reactant_name .
    ?product bp:displayName ?product_name .

    BIND (replace(str(?reaction), 'http://glycosmos.org/biopax/pathway#', '') AS ?reaction_name)
    BIND (replace(str(?reactant_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_names)
    BIND (replace(str(?product_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?product_names)
    BIND (replace(str(?reactant), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_node)
    BIND (replace(str(?product), 'http://glycosmos.org/biopax/pathway#', '') AS ?product_node)
    #BIND (replace(str(?prod_comp), 'http://glycosmos.org/biopax/pathway#', '') AS ?prod_node_name)

    OPTIONAL {   

    ?pathwaystep bp:stepProcess ?catalysis .
    ?catalysis bp:controller ?controller .
    ?controller bp:component ?comp.
    ?comp bp:displayName ?comp_name.
    
    BIND (replace(str(?comp), 'http://glycosmos.org/biopax/pathway#', '') AS ?comp_node)
    BIND (replace(str(?controller), 'http://glycosmos.org/biopax/pathway#', '') AS ?cntrol_node)
    BIND (replace(str(?comp_name), '^^<http://www.w3.org/2001/XMLSchema#string>', '') AS ?comp_node_name)
    }

}
ORDER BY ?reaction

```

## Output
```javascript


({
  json({list}) {
    return list.results.bindings.map((row) => {
      let reaction_num_array = row.reaction_names.value.split(',');
      let reactants_array = row.reactant_names_arry.value.split(',');
      let reactants_name_array = row.reaction_names.value.split(',');
      let products_array = row.product_nodes.value.split(',');
      let products_name_array = row.product_name_arry.value.split(',');
      if (row.comp_nodes){
        control_comp_array =  row.comp_nodes.value.split(',');
      } else {
        control_comp_array = "";
      }
      if (row.comp_names){
        control_comp_nameArray =  row.comp_names.value.split(',');
      } else {
        control_comp_nameArray = "";
      }
      if (row.control_node) { 
        controller_node = row.control_node.value ;
      } else { 
        controller_node = "";
      }
      return {
        "reaction_id": reaction_num_array,
        "reactant_array": reactants_array,
        "reactant_name_array": reactants_name_array,
        "controller": controller_node,
        "cont_comp_array": control_comp_array,
        "cont_comp_displayName": control_comp_nameArray,
        "product_array": products_array,
        "product_name_array": products_name_array
      } 
    });  
  }
})

```