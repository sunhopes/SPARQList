# glycan_resource_upload

## Parameters
    * `glytoucan_text_name`  Input text of glycan structure
## Endpoint
http://path-virtuoso:8890/sparql

## `input_table` 

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX glytoucan: <http://identifiers.org/glytoucan/>
PREFIX glyco: <http://purl.jp/bio/12/glyco/glycan#>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX toucan: <https://glytoucan.org/Structures/Glycans/>
PREFIX : <http://glycosmos.org/biopax/pathway#>

INSERT DATA
{
    ##GRAPH <http://localhost:8890/proteinPathwayUpload>
    #GRAPH <http://localhost:8890/testSpace>
    GRAPH <http://localhost:8890/testSpace2>
    {      
        :{{pw_id}}_GC-{{glycan_node_name}}  bp:type bp:SmallMolecule ;
                                            bp:type glyco:Saccharide ;
                                            obo:GNO_00000022  toucan:{{glytoucan_id}} .
        {{#if glytoucan_text_name }}
           :{{pw_id}}_GC-{{glycan_node_name}} bp:name "{{glytoucan_text_name}}" .
        {{/if}}
    
    }   
 }

```
 ## Output

```javascript
({
  json({input_table}) {
    return input_table.results.bindings
    }
})
```  