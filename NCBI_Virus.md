# NCBI Virus

## Endpoint
http://path-virtuoso:8890/sparql

## `list`

```sparql
PREFIX uni: <http://purl.uniprot.org/core/>
PREFIX taxon: <http://purl.uniprot.org/taxonomy/>

SELECT DISTINCT  ?c_taxon_id  ?c_taxon_name
FROM <http://localhost:8890/uniprotTaxon>
WHERE {
    ?taxon_id rdfs:subClassOf taxon:10239.
    ?c_taxon_id rdfs:subClassOf* ?taxon_id .
    ?c_taxon_id uni:rank uni:Species ;
                uni:scientificName ?c_taxon_name .    
    FILTER (!regex(?c_taxon_name, ("unidentified"))&&!regex(?c_taxon_name, ("ncultured"))&&!regex(?c_taxon_name, ("acterium"))&&!regex(?c_taxon_name, ("endosymbiont"))&&!regex(?c_taxon_name, ("soil"))&&!regex(?c_taxon_name, ("clone")))
}
#limit 10000
```

## Output

```javascript
({
  json({list}) {
    return list.results.bindings.map((row) => {
      // let array = row.pdb_ids.value.split(',')
      return{
        "Taxon_id": row.c_taxon_id.value.replace("http://purl.uniprot.org/taxonomy/",""),
        "Taxon_name": row.c_taxon_name.value
      }
    });
  }
})

```

## Comment
Virus species in NCBI taxon