# Pathway Visualization
pathway visualization with SPV tool

## Parameters
* `pathway_id` Pathway ID

## Endpoint 
http://path-virtuoso:8890/sparql

## `spv_reaction` Query for pathway
```sparql
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX gc: <http://glycosmos.org/biopax/1/562#>

SELECT ?left ?right ?enz_id
FROM <http://localhost:8890/repo_test1>
WHERE {
  #Reactant GlycanIds
  gc:GC-Pathway-{{ pathway_id }} bp:pathwayComponent ?pw_components .
  ?pw_components rdf:type bp:BiochemicalReaction;
                  bp:left ?glycan1;
                  bp:right ?glycan2.
  ?glycan1 bp:xref ?product_glycan1 .
  ?product_glycan1  bp:id ?product_glycan1_id.
  FILTER (!regex(?product_glycan1_id, ("compound")))
  BIND ((REPLACE(str(?product_glycan1_id),"http://identifiers.org/glytoucan/","")) as ?left)
  #Product GlycanIds
  ?glycan2 bp:xref ?product_glycan2 .
  ?product_glycan2  bp:id ?product_glycan2_id.
  BIND ((REPLACE(str(?product_glycan2_id),"http://identifiers.org/glytoucan/","")) as ?right)
    
  #Enzyme
  ?catalysis bp:controlled ?pw_components .
  ?catalysis bp:controller ?enzyme .
  ?enzyme bp:xref ?protein .
  ?protein bp:id ?enz_id.
 
}
```
## Output
```javascript
({
  json({spv_reaction}) {
    return spv_reaction.results.bindings.map((row) => {    
      return {
        "reactant" : row.left.value,
        "product" : row.right.value,
        "controller" : row.enz_id.value
      };
    });
  },
  text({spv_reaction}) {
    return spv.results.bindings.map((row) => {
      return {
        "reactant" : row.left.value,
        "product" : row.right.value,
        "controller" : row.enz_id.value
      };
    });
  }
})
```