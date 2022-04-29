# Identify_glycan_backbone

# Parameter
 
## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT DISTINCT ?glycan_id ?motif
FROM <http://localhost:8890/toucanMotif>
WHERE{
  #?glycan a glycan:Saccharide .
  #?glycan dcterms:identifier ?gtc .
  #BIND(IRI(CONCAT("http://glytoucan.org/Structures/Glycans/", ?gtc)) AS ?glycan_motif)
  ?glycan_id rdfs:label ?motif .
}


