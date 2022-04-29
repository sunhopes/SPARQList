# protein keyword match list (UniProt)

## Parameters
* `input_text`  input keyword
* `input_site`  other input site
## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX up: <http://purl.uniprot.org/core/>

SELECT DISTINCT ?protein_id ?protein_name ?species_name ?dna_name
FROM <http://8890/uniProt>
WHERE
{
 	?protein_id rdfs:label ?protein_name .
      FILTER(CONTAINS(STR(?protein_name), "{{input_text}}"))
 	?protein_id	up:encodedBy ?gene ;
 				up:organism ?taxon .
	?taxon up:scientificName ?species_name .
	?gene skos:prefLabel ?dna_name .
}
```

## Output
```javascript
({
  json({list,input_site}){
    
    return list.results.bindings.map((row) => {
        only_id = row.protein_id.value.replace("http://purl.uniprot.org/uniprot/", "")
        return{
          //"index": "<button type='button' class='btn btn-default' data-dismiss='modal'>Close</button>",
          "index": `<a herf='#' onclick="insert(this,'${input_site.toString()}','${only_id}')">` + row.protein_name.value + "</a>",
          //"index": "<a herf='#' onclick='test(this)'>" + "select" + "</a>",
          "protein_name": row.protein_name.value,
          "protein_uri": row.protein_id.value,
          "protein_id": only_id,
          "taxon_name": row.species_name.value,
          "DNA_name": row.dna_name.value
        }
    });
  } 
})
 ```