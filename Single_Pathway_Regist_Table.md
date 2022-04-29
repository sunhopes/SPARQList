<<<<<<< HEAD
# glycan pathway list(Glycosmos Table)  
=======
# Single Pathway Regist Glycosmos Table  
>>>>>>> d35be139315527dff417baa8719807f3cd17673e

## Parameters
* `order`
  * default: ASC(lcase(?pw_name))
* `limit`
  * default: LIMIT 10
* `offset`
  * default: 0
* `search`
  * default:

## Endpoint
http://path-virtuoso:8890/sparql

## `list`
<<<<<<< HEAD
```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX sio: <http://semanticscience.org/resource/>
SELECT DISTINCT ?pw_id str(?pathway_name) as ?pw_name str(?pathway_category) as ?pw_category ?pathway_category_id str(?pathway_disease) as ?pw_disease (GROUP_CONCAT(DISTINCT ?enzymes_name ; separator = ",") AS ?enzymes_names) (GROUP_CONCAT(DISTINCT ?product_glycan_ids ; separator = ",") AS ?glycan_ids)
FROM <http://localhost:8890/repo_test1>
=======

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

SELECT DISTINCT ?pw_id str(?pathway_name) as ?pw_name str(?pathway_category) as ?pw_category ?pathway_category_id str(?pathway_disease) as ?pw_disease (GROUP_CONCAT(DISTINCT ?enzymes_name ; separator = ",") AS ?enzymes_names) (GROUP_CONCAT(DISTINCT ?product_glycan_ids ; separator = ",") AS ?glycan_ids)
FROM <http://localhost:8890/repo_test1>

>>>>>>> d35be139315527dff417baa8719807f3cd17673e
WHERE{
  # Pathway Name
  ?pw rdf:type bp:Pathway .
  ?pw bp:displayName ?pathway_name .
  # Pathway Category
  ?pw skos:altLabel ?pathway_category .
  # Pathway Category ID
  ?pw rdfs:seeAlso ?pathway_category_id .
  #Pathway Disease
  ?pw sio:SIO_000001 ?disease.
  ?disease rdfs:label ?pathway_disease .
  #Pathway Enzymes
  ?pw bp:pathwayComponent ?pw_components .
  ?pw_components rdf:type bp:Catalysis;
                 bp:controller ?protein. 
  ?protein bp:displayName ?enzymes_name .
  #Product GlycanIds
  ?pw bp:pathwayComponent ?pw_components1 .
  ?pw_components1 rdf:type bp:BiochemicalReaction;
                  bp:right ?glycan. 
  ?glycan bp:xref ?product_glycan .
  ?product_glycan  bp:id ?product_glycan_id.
  BIND (replace(str(?product_glycan_id), 'http://identifiers.org/glytoucan/', '') AS ?product_glycan_ids)
  BIND (replace(str(?pw), 'http://glycosmos.org/biopax/1/562#GC-Pathway-', '') AS ?pw_id)
<<<<<<< HEAD
  {{search}}
=======
  
  {{search}}
  
>>>>>>> d35be139315527dff417baa8719807f3cd17673e
  }
ORDER BY {{order}}
{{limit}}
OFFSET {{offset}} 
```

## Output
```javascript
({
  json({list}){
    return list.results.bindings.map((row) => {
      let glycan_array = row.glycan_ids.value.split(',')
      let enzyme_array = row.enzymes_names.value.split(',')
      return{
        "pathway_id": row.pw_id.value,
        "pathway_name": row.pw_name.value,
        "pathway_category": row.pw_category.value,
        "pathway_category_id": row.pathway_category_id.value.replace("http://purl.obolibrary.org/obo/PW_",""),
        "pathway_disease": row.pw_disease.value,
        "pathway_glycanid": glycan_array,
        "enzyme_name_array": enzyme_array
      }
    });
  }
})
```