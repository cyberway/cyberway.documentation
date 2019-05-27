
# 4 Understanding ABI Files  

The Application Binary Interface (ABI) is a JSON-description of the methods for transforming user actions between JSON and binary representations. ABI also describes methods for converting database state to JSON-format and vice versa. After describing the contract through ABI, developers and users can easily interact with it via JSON.  

To create an ABI file, use the eosio-cpp utility located in cyberway.cdt. Sometimes, for debugging purposes or for analysis, it is necessary to modify manually an already created ABI file. Therefore, the developers need to familiarize themselves with its structure, as well as with a description of its individual components. The structure of an ABI file is the following:

``` 
{
   "version": "cyberway::abi/1.1",
   "structs": [],
   "types": [],
   "actions": [],
   “events”: [],
   "tables": [],
   "variants": [],
   “abi_extensions”: []
}
```  
**structs** — a description of the structures used to process the actions of the user. The structures can also be used to describe the fields of objects that are stored in tables or the fields (parameters) of functions in actions.  

**types** — a description of user types used as parameters in any public action of the user or structure. It can also be used across public actions or structures. You may also describe your own types by yourself.  

**actions** — a description of the public actions of the user that combines all the functions described in the header file of the contract.  

**events** — a description of events, each of which represents a reaction (response) of a contract to a specific action of the user.  

**tables** — a description of the tables used to process the actions of the user. The tables are objects of the type `multi_index` or `eosio::singleton`, which store data in the database of the blockchain node. Inside the table are `structs`. Creating, receiving, modifying or deleting objects is performed in `actions`.  

**abi_extensions** — a description of additional ABI features (reserved, not used).   

## Structs
An example of a structure description in an ABI file: 
```
{
   "name": "issue", // The name 
   "base": "", 	    // Inheritance, parent struct
   "fields": []	    // Array of field objects describing the struct's fields
}
```  
An example of a structure field description in an ABI file:
```
{
   "name":"",       // The field's name 
   "type":""        // The field's type
}   
```
An example of a structure of a `create` operation for creating tokens in an ABI file:
```
{
  "name": "create",      
  "base": "",           

  "fields": [
    {
      "name":"issuer",  	// Token creator name
      "type":"name"      
    },
    {
      "name":"maximum_supply", // Maximum number of tokens 
      "type":"asset"
    }
  ]
}
```
## Types
Пример описания типа в ABI-файле:
```
{
   "new_type_name": "name",
   "type": "name"
}
```

## Actions
An example of a type description in an ABI file:

```
{
  "name": "transfer", 	// The name of the action as defined in the contract
  "type": "transfer" 	// The name of the implicit struct as described in the ABI
}
```
## Events
An example of the description of events for a publishing contract in an ABI file:
```
"events": [
	{"name": "poolstate", "type": "pool_event"},
	{"name": "poolerase", "type": "uint64"},
	{"name": "postcreate","type": "create_event"},
	{"name": "poststate", "type": "post_event"},
	{"name": "postclose", "type": "post_close"},
	{"name": "votestate", "type": "vote_event"}
  ],
```

## Tables
An example of the description of a table in an ABI file:
```
{
    "name": "emitparams",
    "type": "emit_state",
    "indexes" : [{
        "name": "primary",
        "unique": true,
        "orders": [{"field": "id", "order": "asc"}]
    }]
}
```

## Vectors’ description
At first, you have to add a type using `[]` characters to describe a vector in an ABI-file. The ABI file tables must be accurately described. For example, in the case of cleos when adding a table to a contract with a distorted ABI description data may be lost if there is no error message popping out.


> You have to update the ABI file straight after a single change is made in the contract structure, tables, actions or events, as well as after introduction of a new type. In most cases if the ABI file update fails, an error message may not appeared. Such situation is difficult to analyze the work of the contract.


****  
