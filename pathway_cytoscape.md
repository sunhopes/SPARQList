# Pathway view with Cytoscape

## Parameters
* `pw_id` pathway_id 

## Endpoint
http://path-virtuoso:8890/sparql

## `list`
```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX gcpw: <http://glycosmos.org/biopax/pathway#>

SELECT DISTINCT  (GROUP_CONCAT(DISTINCT ?reaction_name ; separator = ",") AS ?reaction_names) (GROUP_CONCAT(DISTINCT ?reactant_node ; separator = ",") AS ?reactant_nodes) (GROUP_CONCAT(DISTINCT ?reactant_names ; separator = ",") AS ?reactant_names_array) ?cont_node ?compl_displayName (GROUP_CONCAT(DISTINCT ?comp_node ; separator = ",") AS ?comp_nodes) (GROUP_CONCAT(DISTINCT ?comp_node_name; separator = ",") AS ?comp_names) (GROUP_CONCAT(DISTINCT ?product_node_names ; separator = ",") AS ?product_nodes) (GROUP_CONCAT(DISTINCT ?products_name ; separator = ",") AS ?product_name_array) 
FROM <http://localhost:8890/testSpace2>
WHERE{ 
    gcpw:GC-Pathway{{pw_id}} bp:pathwayOrder ?pathwaystep .
    ?pathwaystep bp:stepProcess ?reaction .
    ?reaction rdf:type bp:BiochemicalReaction .
    ?reaction bp:left ?reactant .
    ?reaction bp:right ?product_node .
    ?reactant bp:displayName ?reactant_name .
    ?product_node bp:displayName ?product_name .
      BIND (replace(str(?reaction), 'http://glycosmos.org/biopax/pathway#', '') AS ?reaction_name)
      BIND (replace(str(?reactant_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_names)
      BIND (replace(str(?reactant), 'http://glycosmos.org/biopax/pathway#', '') AS ?reactant_node)
      BIND (replace(str(?product_node), 'http://glycosmos.org/biopax/pathway#', '') AS ?product_node_names)
      BIND (replace(str(?product_name), 'http://glycosmos.org/biopax/pathway#', '') AS ?products_name)
    OPTIONAL {   
      ?pathwaystep bp:stepProcess ?catalysis .
      ?catalysis bp:controller ?control_node .
      ?control_node bp:component ?comp .
      ?control_node bp:displayName ?compl_displayName .
      ?comp bp:displayName ?comp_name .
        BIND (replace(str(?comp), 'http://glycosmos.org/biopax/pathway#', '') AS ?comp_node)
        BIND (replace(str(?control_node), 'http://glycosmos.org/biopax/pathway#', '') AS ?cont_node)
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
      let reaction_num = row.reaction_names.value;
      let reactants_array = row.reactant_nodes.value;
      let reactant_names = row.reactant_names_array.value;
      let products_array = row.product_nodes.value;
      let product_names = row.product_name_array.value;
      if (row.cont_node) { 
        catalysis_node = row.cont_node.value ;
      } else { 
        catalysis_node = "";
      }
      if (row.comp_nodes) { 
        //cont_comp_nodes = row.comp_nodes.value.split(',');
        cont_comp_nodes = row.comp_nodes.value;
      } else { 
        cont_comp_nodes = "";
      }
      if (row.compl_displayName) { 
        cont_comp_name = row.compl_displayName.value;
      } else { 
        cont_comp_name = "";
      }
      return {
        "reaction_id": reaction_num,
        "reactant_nodes": reactants_array,
        "reactant_displayNames": reactant_names,
        "controller": catalysis_node,
        "control_displayName": cont_comp_name,
        "cont_comp_array": cont_comp_nodes,
        "product_nodes": products_array,
        "product_displayNames": product_names
      } 
    });  
  }
})

```