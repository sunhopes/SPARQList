# test_upload_function

## Parameters
* `pw_id` pathway id
* `test1` test id
* `test2` test id
## Endpoint
http://path-virtuoso:8890/sparql

## `input_table` 

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX bp: <http://www.biopax.org/release/biopax-level3.owl#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX : <http://glycosmos.org/biopax/pathway#>
INSERT DATA
{
  GRAPH <http://localhost:8890/testSpace>  
  ##GRAPH <http://localhost:8890/proteinPathwayUpload>
    {
     
	:GC-"{{test1}}" rdf:type bp:Pathway .
    :GC-"{{test2}}" rdf:type bp:Pathway .
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