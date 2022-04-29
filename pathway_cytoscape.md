# Pathway view with Cytoscape

## Parameters
* `pw_id` get pathway_id from pathway list(glycosmos webcomponent)

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>

SELECT DISTINCT  (GROUP_CONCAT(DISTINCT ?reaction_name ; separator = ",") AS ?reaction_names) (GROUP_CONCAT(DISTINCT ?reactant_node ; separator = ",") AS ?reactant_nodes) (GROUP_CONCAT(DISTINCT ?reactant_names ; separator = ",") AS ?reactant_names_array) ?control_node (GROUP_CONCAT(DISTINCT ?comp_node ; separator = ",") AS ?comp_nodes) (GROUP_CONCAT(DISTINCT ?comp_node_name; separator = ",") AS ?comp_names) ?product_node_name ?product_name 
FROM <http://localhost:8890/testSpace2>
WHERE{ 
    ?pw_id bp:pathwayOrder ?pathwaystep .
    ?pathwaystep bp:stepProcess ?reaction .
    ?reaction rdf:type bp:BiochemicalReaction .
    ?reaction bp:left ?reactant .
    ?reaction bp:right ?product_node .
    ?reactant bp:displayName ?reactant_name .
    ?product_node bp:displayName ?product_name .
      BIND (replace(str(?reaction), 'http://glycosmos.org/biopax/pathway#', '') AS ?reaction_name)
      BIND (replace(str(?reactant_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_names)
      BIND (replace(str(?reactant), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_node)
      BIND (replace(str(?product_node), 'http://glycosmos.org/biopax/pathway#', '') AS ?product_node_name)
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
      let reactants_array = row.reactant_nodes.value.split(',');
      let reactant_names = row.reactant_names_array.value.split(',');
      let product = row.product_node_name.value;
      let product_name = row.product_name.value;
      if (row.control_node) { 
        catalysis_node = row.control_node.value ;
      } else { 
        catalysis_node = "";
      }
      if (row.comp_nodes) { 
        cont_comp_nodes = row.comp_nodes.value.split(',');
      } else { 
        cont_comp_nodes = "";
      }
      if (row.comp_names) { 
        cont_comp_names = row.comp_names.value.split(',');
      } else { 
        cont_comp_names = "";
      }
      return {
        "reaction_id": reaction_num_array,
        "reactant_array": reactants_array,
        "reactant_name_array": reactant_names,
        "controller": catalysis_node,
        "cont_comp_array": cont_comp_nodes,
        "cont_comp_displayName": cont_comp_names,
        "product_node": product,
        "product_name": product_name
      } 
    });  
  }
})

```