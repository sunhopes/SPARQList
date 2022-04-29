# PubChem で薬を薬効で分類（FDA Approved Drugs → WHO ATC Code）(フロント開発用 with hasChild flag.)（建石, 守屋）

- hasChild 入り
- フロント開発用なので、あとで元のと統合

- 入力：
  - ATCコードのカテゴリ。（デフォルトは全部：このばあい、パラメータは空白)
- 出力：
  - 入力したATCコードのカテゴリのサブカテゴリに含まれるPubChem Compound ID数（サブカテゴリ単位で集計）
  - 一つの薬に複数のATCコードがついていることがありうる 

## Endpoint

https://integbio.jp/rdf/pubchem/sparql

## Parameters

* `categoryIds`   ATCコード、デフォルトは空。指定された場合、その下の内訳。
  *  第1階層：英大文字１ケタ, 第2階層：数字２ケタ, 第３階層：英大文字１ケタ, 第４階層：英大文字１ケタ, 第５階層：数字２ケタ （個別の薬）  
  * default:  
  * example: J (ANTIINFECTIVES FOR SYSTEMIC USE), J05 (ANTIVIRALS FOR SYSTEMIC USE), J05A (DIRECT ACTING ANTIVIRALS), J05AE (Protease inhibitors)
* `queryIds` PubChem Concept ID (数字のみ）、空白の場合はFDA Approved Drugsであるもの全体
  * default: 
  * example: 3561, 6957673, 3226, 452548, 19861, 41781, 4909, 15814656, 13342, 11597698, 3396, 60937, 86767262, 43507, 3342, 4642, 5311497, 3356, 37464, 5353853 
* `mode`
  * example: idList, objectList

## `queryArray`
- ユーザが指定した ID リストを配列に分割
```javascript
({queryIds}) => {
  queryIds = queryIds.replace(/,/g," ")
  if (queryIds.match(/[^\s]/)) return queryIds.split(/\s+/);
  return false;
}
```

## `categoryArray`
- category ID を配列に分割
```javascript
({categoryIds}) => {
  categoryIds = categoryIds.replace(/,/g," ")
  if (categoryIds.match(/[^\s]/)) return categoryIds.split(/\s+/);
  return false;
}
```

## `data`

```sparql
PREFIX sio: <http://semanticscience.org/resource/>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX compound: <http://rdf.ncbi.nlm.nih.gov/pubchem/compound/CID>
PREFIX concept: <http://rdf.ncbi.nlm.nih.gov/pubchem/concept/ATC_>
PREFIX pubchemv: <http://rdf.ncbi.nlm.nih.gov/pubchem/vocabulary#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
{{#if mode}}
SELECT DISTINCT ?cid ?category ?label
{{else}}
SELECT ?category ?label ?count (SAMPLE (?child_atc) AS ?child)
WHERE {
  {
    SELECT ?category ?label (COUNT (DISTINCT ?cid) AS ?count)
{{/if}}
    WHERE {
      {{#if queryArray}}
      VALUES ?cid { {{#each queryArray}} compound:{{this}} {{/each}} }
      {{else}}
      ?cid      obo:has-role pubchemv:FDAApprovedDrugs .   #  これがないとタイムアウトする位遅いので    
      {{/if}}
      {{#if categoryArray}}
        {{#if mode}}
      VALUES ?category
        {{else}}
      VALUES ?parent
        {{/if}}
          { {{#each categoryArray}} concept:{{this}} {{/each}} }
      {{/if}}
 
      ?attr a sio:CHEMINF_000562 ;
            sio:is-attribute-of ?cid ; 
            sio:has-value  ?WHO_INN ;
            dcterms:subject ?WHO_ATC .
      filter(contains(str(?WHO_ATC), "http://rdf.ncbi.nlm.nih.gov/pubchem/concept/ATC"))
      ?WHO_ATC skos:broader* ?category .
      {{#if categoryArray}}
        {{#unless mode}}
      ?category skos:broader  ?parent.
        {{/unless}}
      {{else}}                     
      filter(regex(str(?category), "http://rdf.ncbi.nlm.nih.gov/pubchem/concept/ATC_[A-Z]$"))
      {{/if}}  
      ?category skos:prefLabel  ?label .
{{#unless mode}}
    } group by ?label ?category
  }
  OPTIONAL { # 内訳時の hasChild のフラグ用 （サブクエリでくくらないと、FILTERとOPTIONALでバグる。mode=*Listのときはサブクエリにしない）
    ?child_atc skos:broader ?category .
  }
{{/unless}}
}

```

## `return`
- 整形
```javascript
({mode, data})=>{
  const idVarName = "cid";
  const idPrfix = "http://rdf.ncbi.nlm.nih.gov/pubchem/compound/";
  const categoryPrefix = "http://rdf.ncbi.nlm.nih.gov/pubchem/concept/ATC_";
  if (mode == "objectList") return data.results.bindings.map(d=>{
    return {
      id: d[idVarName].value.replace(idPrfix, ""), 
      attribute: {
        categoryId: d.category.value.replace(categoryPrefix, ""), 
        uri: d.category.value,
        label : d.label.value
      }
    }
  });
  return data.results.bindings.map(d=>{
    return {
      categoryId: d.category.value.replace(categoryPrefix, ""), 
      label: d.label.value,
      count: d.count.value,
      hasChild: Boolean(d.child)
    };
  });	
}
```