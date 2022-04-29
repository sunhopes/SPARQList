# Protein related pathway Search

## Parameters
	* `order`
  	* default: ASC(lcase(?ppw_name)), DESC(lcase(?reaction))
* `limit`
  * default: LIMIT 50
* `offset`
  * default: 0
* `search`
  * default: 

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
SELECT DISTINCT ?ppw ?ppw_name ?reaction (GROUP_CONCAT(DISTINCT ?re_name ; separator = ",") AS ?re_names) ?cal_name (GROUP_CONCAT(DISTINCT ?prod_name ; separator = ",") AS ?prod_names)
FROM <http://localhost:8890/testSpace>
WHERE {
  ?ppw rdf:type bp:Pathway.
  ?ppw bp:displayName ?ppw_name .
  ?ppw bp:pathwayComponent ?reaction.
  ?reaction bp:left ?reactant.
  ?reactant bp:displayName ?re_name .
  ?reaction bp:right ?product.
  ?product bp:displayName ?prod_name .
  OPTIONAL { ?catalysis bp:controlled ?reaction .
             ?catalysis bp:controller ?reaction_true .
             ?reaction_true bp:displayName ?cal_name
            }
  FILTER (?ppw NOT IN(<http://glycosmos.org/biopax/pathway#GC-Pathway100>))
  {{search}}
  }
ORDER BY ?ppw ?reaction
{{limit}}
OFFSET {{offset}} 
```

## Output
```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      let pathway_id = row.ppw.value.replace("http://glycosmos.org/biopax/pathway#", "")
      let reactant_array = row.re_names.value.split(',')
      let product_array = row.prod_names.value.split(',')
      if (row.cal_name) { 
        controller = row.cal_name.value;
      } else {
        controller = "";
      }
        
      return{
        "protein_pw_id": `<a herf='#' onclick="pwid_link(this)">` + pathway_id + "</a>",
        "protein_pw_name": row.ppw_name.value,
        "protein_reaction_num": row.reaction.value.replace("http://glycosmos.org/biopax/pathway#", ""), 
        "protein_pw_reactant": reactant_array,
        "protein_pw_cntroller": controller,
        "protein_pw_product": product_array
      }  
    });
  }
})
```
