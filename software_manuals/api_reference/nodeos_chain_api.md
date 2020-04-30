# Nodeos Chain API

API provides access to the blockchain information and interaction with the blockchain.
For API request, it needs to perform an `POST` line with required parameters. The manual presents examples of requests using `curl`.
Return code *200* indicates successful operation.

**API requests supported:**
  * [get_account](#get_account)
  * [get_block](#get_block)
  * [get_code](#get_code)
  * [get_info](#get_info)
  * [get_code_hash](#get_code_hash)
  * [get_block_header_state](#get_block_header_state)
  * [get_abi](#get_abi)
  * [get_raw_code_and_abi](#get_raw_code_and_abi)
  * [get_raw_abi](#get_raw_abi)
  * [get_table_rows](#get_table_rows)
  * [get_currency_balance](#get_currency_balance)
  * [get_currency_stats](#get_currency_stats)
  * [get_producers](#get_producers)
  * [get_producer_schedule](#get_producer_schedule)
  * [get_scheduled_transactions](#get_scheduled_transactions)
  * [abi_json_to_bin](#abi_json_to_bin)
  * [abi_bin_to_json](#abi_bin_to_json)
  * [get_required_keys](#get_required_keys)
  * [get_transaction_id](#get_transaction_id)
  * [get_agent_public_key](#get_agent_public_key)
  * [resolve_names](#resolve_names)
  * [get_proxy_status](#get_proxy_status)

## get_account
The request returns an object containing various details about a specific account on the blockchain.

**Params:**
  * `(name) account_name` — An account that data is being requested for.
  * `(symbol) expected_core_symbol` — A token name, consisting of a set of capital letters. This field is optional.


**Request examples:**  
```
curl --request POST  -d '{"account_name" : "rows"}' <node>/v1/chain/get_account
```
```
curl --request POST  -d '{"account_name" : "cyber"}' <node>/v1/chain/get_account
```
```
curl --request POST  -d '{"account_name":"alice", "symbol":"SYS"}' <node>/v1/chain/get_account
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "account_name": "string",        // Account information about which is issued
  "head_block_num": 0,             // Hashed number of the last block created
  "head_block_time": "string",     // Date and time the last block was created (YYYY-MM-DDTHH:MM:SS.sss)
  "privileged": false,
  "last_code_update": "string",    // Date and time of the last code modification (YYYY-MM-DDTHH:MM:SS.sss)
  "created": "string",             // Point creation date and time (YYYY-MM-DDTHH:MM:SS.sss)
  "core_liquid_balance": "string", // A string representation of tokens, composed of a float with a precision of 4, and a symbol composed of capital letters between 1-7 letters separated by a space, example 1.0000 ABC.

  "ram_quota": 0,                  // A whole number
  "net_weight": 0,                 // A whole number
  "cpu_weight": 0,                 // A whole number

  "net_limit": "string",           // Net available resources
  "cpu_limit": "string",           // CPU available resources
  "ram_limit": "string",           // RAM available resources
  "storage_limit": "string",       // Storage available resources

  "ram_usage": 0,                  // Amount of memory used

  "permissions": "prods",
  "total_resources": {...},
  "self_delegated_bandwidth": {...},
  "refund_request": {...},
  "voter_info": {...},

  "stake_info": "string"            // Number of staked tokens on account balance
}
```

## get_block
The request returns an object containing various details about a specific block on the blockchain.

**Params:**
  * `(string) block_num_or_id` — A block number or a block id.

**Request example:**  
```
curl --request POST  -d '{"block_num_or_id": "6707315"}' <node>/v1/chain/get_block
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "timestamp": "2020-04-20T16:28:09.000", // Date and time the block was created (YYYY-MM-DDTHH:MM:SS.sss)
  "producer": "string",                   // Account of validator who signed the block
  "confirmed": 21,                        // Number of validators confirming the block
  "previous": "006...9bd",                // Previous block ID
  "transaction_mroot": "1434...d09",      // Parent transaction ID
  "action_mroot": "dea...469",            // Parent action ID
  "schedule_version": 59218,
  "new_producers": null,                  // Account names of the new validators
  "header_extensions": [],
  "producer_signature": "SIG_K1_J...eR",
  "transactions": [                       // array of transactions included in the block
    {
      "status": "executed",               // Transaction status, takes one of the following values:
                                              "executed" - successful execution, lack of error messages;
                                              "soft_fail" - unsuccessful execution; errors that appeared during the process were processed;
                                              "hard_fail" - unsuccessful execution; errors that appeared during the process could not be processed;
                                              "delayed" - transaction execution is postponed;
                                              "expired" - the transaction timed out, the resources allocated for the transaction were returned to the owner
      "cpu_usage_us": 33823,              // CPU resource allocated for each transaction
      "net_usage_words": 88,              // Network resource allocated for each transaction
      "ram_kbytes": 19,                   // RAM resource allocated for each transaction
      "storage_kbytes": 0,                // Storage resource allocated for each transaction
      "trx": {                            // A transaction included in the block
        "id": "17...8d",
        "signatures": [
          "SIG_K1_K...24d",
          "SIG_K1_K...Fr3"
        ],
        "compression": "none",
        "packed_context_free_data": "",
        "context_free_data": [],
        "packed_trx": "a5cd...4000",
        "transaction": {...}
      }
    }
  ],
  "block_extensions": [],
  "id": "006...58f22d10",                  // The block ID
  "block_num": 6707315,                    // Number of the block
  "ref_block_prefix": 744221143
}

```

## get_code
The request returns an object containing rows from the table for a specified account.  

**Params:**
  * `(name) account_name` — Account name on which the contract is based.
  * `(bool) code_as_wasm` — Binary code. Default is `false`.

**Request example:**  
```
curl --request POST  -d '{"account_name": "string", "code_as_wasm": 1}' <node>/v1/chain/get_block
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "account_name": "string",
  "wast": "string",         // Textual information (WAST from get_code is no longer supported)
  "wasm": "string",         // Binary code
  "code_hash": "string",
  "abi": {                  // ABI object
    "____comment": "string",
    "version": "string",
    "types": [
      {...}
    ],
    "structs": [
      {...}
    ],
    "actions": [
      {...}
    ],
    "events": [
      {...}
    ],
    "tables": [
      {...}
    ],
    "variants": []
  }
}

```

## get_info
The request returns an object containing various details about the blockchain.  

**Params:**  
No params required.  

**Request example:**  
```
curl --request POST --data '' <node>/v1/chain/get_info
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "server_version": "string",       // Hash code representing the last commit in the marked version
  "chain_id": "string",             // Hash string of a chain ID
  "head_block_num": 0,              // A number of the last received block in chain
  "last_irreversible_block_num": 0, // A number of the last block in a chain that is loaded into the system state
  "last_irreversible_block_id": "string", // Hash identifier of the last block in a chain that is loaded into the system state
  "head_block_id": "string",        // Identifier of the last block in a chain
  "head_block_time": "string",      // The time when the last block appeared in a chain
  "head_block_producer": "string",  // Block producer who signed the last block in a chain
  "virtual_block_cpu_limit": 0,     // Processor limit calculated after each block
  "virtual_block_net_limit": 0,     // Network limit calculated after each block
  "block_cpu_limit": 0,             // Actual maximum processor limit
  "block_net_limit": 0,             // Actual maximum network limit
  "server_version_string": "string" // String representation of a server version (patch version)
}
```

## get_code_hash
The request returns hash code of an object containing rows from the table for a specified account.  

**Params:**
  * `(name) account_name` — An account for which information in form of hash code is requested.

**Request example:**  
```
curl --request POST  -d '{"account_name": "cyber.stake"}' <node>/v1/chain/get_code_hash
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "account_name": "cyber.stake",
  "code_hash": "3f5f05119...9d1c73a08b1"
}
```

## get_block_header_state
The request retrieves the block header state. This query can only be applied to a block that is not irreversible.  

**Params:**
  * `string block_num_or_id` — A block_number or a block_id.

**Request example:**  
```
 curl --request POST -d '{"block_num_or_id" : "006762...9c283"}' <node>/v1/chain/get_block_header_state
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "id":"006762...9c283",                         // Block ID
  "block_num":6775398,                           // Block number
  "header":{
    "timestamp":"2020-04-23T05:21:39.000",     // Time when the block appeared
    "producer":"string",                       // Account of the validator who created the block
    "confirmed":38,                            // Number of validators confirming the block
    "previous":"006762...860fd5",              // Previous block ID
    "transaction_mroot":"0000...0000",         // Parent transaction ID
    "action_mroot":"97ee40...98a8",            // Parent action ID
    "schedule_version":59678,                  // Schedule version
    "header_extensions":[],                    // Additional information
    "producer_signature":"SIG_K1_J...e9N"      // A signature of the validator who created the block
  },
  "dpos_proposed_irreversible_blocknum":6775366,
  "dpos_irreversible_blocknum":6775334,
  "bft_irreversible_blocknum":0,
  "pending_schedule_lib_num":6775337,
  "scheduled_shuffle_slot":213644813,
  "pending_schedule_hash":"d55bd...f4b6",
  "pending_schedule":{
    "version":59679,
    "producers":[                              // Validators that are in the pending schedule
      {"producer_name":"string","block_signing_key":"GLS7X...9ZpL"},
      {...}
    ]
  },
  "active_schedule":{
    "version":59678,
    "producers":[                              // Validators that are in the active schedule
      {"producer_name":"string","block_signing_key":"GLS7By...vt9s"},
      {...}
    ]
  },
  "blockroot_merkle":{
    "_active_nodes":[                          // Active node list
      "006762...60fd5",
      "..."
    ],
    "_node_count":6775397
  },
  "producer_to_last_produced":[
    ["string",6775384],                        // Account of a validator and the last block it produced
    [...]
  ],
  "producer_to_last_implied_irb":[
    ["string",6775356],                        // Account of a validator and the last irreversible block it implied
    [...]
  ],
  "block_signing_key":"GLS8...QU2u",
  "confirm_count":[1,1,2,2,2,3,...,21,22,23,24], // Number of confirmed blocks
  "confirmations":[]
}
```

## get_abi
The request retrieves the ABI for a contract based on its account name.  

**Params:**
  * `(name) account_name` — Account on which the contract is based.

**Request example:**  
```
curl --request POST  -d '{"account_name": "string"}' <node>/v1/chain/get_abi
```

### Responses
**Code:** 200 OK  

**Value:**
```
  "account_name": "string",   // Account on which the contract is based
  "abi": {                    // ABI object
    "____comment": "string",
    "version": "string",
    "types": [
      {...}
    ],
    "structs": [
      {...}
    ],
    "actions": [
      {...}
    ],
    "events": [
      {...}
    ],
    "tables": [
      {...}
    ],
    "variants": []
  }
}
```

## get_raw_code_and_abi
The request retrieves raw code and ABI for a contract based on account name.  

**Params:**
  * `(name) account_name` — Account on which the contract is based.

**Request example:**  
```
curl --request POST  -d '{"account_name": "string"}' <node>/v1/chain/get_raw_code_and_abi
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "account_name": "string", // Account on which the contract is based
  "wasm": "string",         // base64 encoded WASM
  "abi": "string"           // base64 encoded ABI
}
```

## get_raw_abi
The request returns an object containing rows from the specified table.

**Params:**
  * `(name) account_name` — Account on which the contract is based.
  * `(sha256) abi_hash` — ABI hash. This field is optional.

**Request example:**  
```
curl --request POST  -d '{"account_name": "string"}' <node>/v1/chain/get_raw_abi
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "account_name": "string", // Account on which the contract is based
  "code_hash": "string",    // Code hash
  "abi_hash": "string",     // ABI hash 
  "abi": "string"           // base64 encoded ABI.  
}
```

## get_table_rows
The request returns an object containing rows from the specified table as well as containing values of the table fields specified in the parameters.  

**Params:**
  * `(bool) json` — Whether the object is converted to JSON. Default is `false`.
  * `(name) code` — The name of the smart contract that controls the provided table.
  * `(string) scope` — The account to which this data belongs.
  * `(name) table` — The name of the table to query.
  * `(string) table_key` — Type of key specified by index_position.
  * `(variant) lower_bound` — Lower bound of search.
  * `(variant) upper_bound` — Upper bound of search.
  * `(uint32_t) limit` — Total number of table rows to retrieve. Default is `10`.
  * `(name) index` — Position of the index used.
  * `(string) encode_type{"dec"}` — The field takes the values `dec`, `hex`. Default is `dec`.
  * `(optional<bool>) reverse` — `true` if the search is in the reverse order. Default is `false`.
  * `(optional<bool>) show_payer` — Display a RAM payer. Default is `false`.


**Request example:**  
```
curl --request POST  -d '{"json": false, "code":"cyber.token", "scope":"cyber", "table" : "accounts", "table_key" : "", "lower_bound" : "", "upper_bound" : "", "limit" : "7", "key_type" : "int64", "index" : "primary", "encode_type" : "dec", "reverse" : false, "show_payer" : true}' <node>/v1/chain/get_table_rows
```
```
curl --request POST  -d '{"json": false, "code":"rows", "scope":"rows", "table" : "values", "table_key" : "", "upper_bound" : {"secondary" : 252, "forth" : 18}, "limit" : "20", "key_type" : "int64", "index" : "multy", "encode_type" : "dec", "reverse" : false, "show_payer" : true}' <node>/v1/chain/get_table_rows
```
```
curl --request POST  -d '{"json": false, "code":"rows", "scope":"rows", "table" : "values", "table_key" : "", "limit" : "5", "key_type" : "", "index" : "primary", "upper_bound" : {"key" : 14}, "encode_type" : "dec", "reverse" : false, "show_payer" : false}' <node>/v1/chain/get_table_rows
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
    vector<fc::variant> "rows": [ // One row per item, either encoded as hex String or JSON object.
        null
    ],
    "more": false,                // `true` if the next element is not finite and if the condition 'sizeof data() < limit' is fulfilled. By default is "false".
    "next": false                 // If `more` field is `true`, this field contains an element located after the last in the rows field.
}
```

## get_currency_balance
The request retrieves the current balance.  

**Params:**
  * `(name) code` — Token code.
  * `(name) account` — Account for which balance is requested.
  * `(optional<string>) symbol` — A string representation of a token symbol, composed of a float with a precision of 4, and a symbol composed of capital letters, for example `1.0000 SYS`.

**Request example:**  
```
curl --request POST  -d '{"code":"cyber.token", "account":"bob" , "symbol" : "SYS"}' <node>/v1/chain/get_currency_balance
```

### Responses
**Code:** 200 OK  

**Value:**
```
{[
    "string"
]}
```

## get_currency_stats
The request retrieves currency stats of a certain type of tokens in the system.  

**Params:**
  * `(name) code` — Token code.
  * `(string) symbol` — A string representation of a token symbol, composed of a float with a precision of 4, and a symbol composed of capital letters between 1-7 letters separated by a space, example `1.0000 SYS`.

**Request example:**  
```
curl --request POST  -d '{"code":"cyber.token", "account":"bob" , "symbol" : "SYS"}' <node>/v1/chain/get_currency_stats
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "supply": "string",             // Specified type tokens in circulation
  "max_supply": "integer",        // Limit of the specified type tokens in circulation
  "account_name issuer": "string" // An account that created tokens of the specified type.
}
```

## get_producers
The request retrieves producers (validators) list.  

**Params:**
  * `(bool) json` — Whether the producers list is converted to JSON. Default is `false`.
  * `(string) lower_bound` — In conjunction with limit can be used to paginate through the results. For example, `limit=10` and `lower_bound=10` would be page `2`.
  * `(uint32_t) limit` — Total number of producers to retrieve. Default is `50`.

**Request example:**  
```
curl --request POST  -d '{"json" : false, "lower_bound" : 2, "limit" : 10 }' <node>/v1/chain/get_producers
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "rows":[                                 // One row per item, either encoded as hex string or JSON object
    {...}
  ],
  "total_producer_vote_weight": "integer",
  "more": "string"                         // Fill lower_bound with this value to fetch more rows
}
```

## get_producer_schedule
The request retrieves producers (validators) list.  

**Params:**  
No params required.  

**Request example:**  
```
curl --request POST --data ''  <node>/v1/chain/get_producer_schedule
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
    "active": [{
        "version": 0,
        "producers": [{
            "producer_name": "string",
            "block_signing_key": "string"
        }]
    }],
    "pending": [{
        "version": 0,
        "producers": [{
            "producer_name": "string",
            "block_signing_key": "string"
        }]
    }],
    "proposed": [{
        "version": 0,
        "producers": [{
            "producer_name": "string",
            "block_signing_key": "string"
        }]
    }]
}
```

## get_scheduled_transactions
The request retrieves deferred transactions.  

**Params:**
  * `(bool) json` — Whether the packed transaction is converted to JSON. Default is `false`.
  * `(string) lower_bound` — Date/time string in the format `YYYY-MM-DDTHH:MM:SS.sss` OR transaction ID.
  * `(uint32_t) limit` — Total number of transactions to retrieve. Default is `50`.

**Request example:**  
```
curl --request POST  -d '{"json" : false, "limit" : 10 }' <node>/v1/chain/get_scheduled_transactions
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
    "transactions": [
        {...}
        "more": "string" // fill lower_bound with this to fetch next set of transactions
    ]
}
```

## abi_json_to_bin
The request returns an object containing rows from the specified table.  

**Params:**
  * `(name) code` — A name of row.
  * `(name) action` — Action name.
  * `(variant) args` — Arguments in JSON form.

**Request example:**  
```
curl --request POST  -d '{"code" : "rows", "action" : "filltable", "args" : {"fildltable" : 30} }' <node>/v1/chain/abi_json_to_bin
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "binargs": "string"
}
```

## abi_bin_to_json
The request returns an object containing rows from the specified table.  

**Params:**
  * `(name) code` — A name of row.
  * `(name) action` — Action name.
  * `(vector<char>) binargs` — String containing binary arguments.

**Request example:**  
```
curl --request POST  -d '{"code" : "rows", "action" : "filltable", "binargs" : "1e00000000000000" }' <node>/v1/chain/abi_bin_to_json
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
    "string"
}
```

## get_required_keys
The request returns the required keys needed to sign a transaction.  

**Params:**
  * (object) transaction:
    * `(string) expiration` — Time in format of `YYYY-MM-DDTHH:MM:SS.sss` that transaction must be confirmed by.  
    * `(integer) ref_block_num` — Block number containing the transaction.
    * `(integer) ref_block_prefix` — Prefix of block containing the transaction.
    * `(string/integer)  max_net_usage_words` — Network resource allocated for the transaction.
    * `(string/integer) max_cpu_usage_ms` — CPU resource allocated for the transaction.
    * `(integer) max_ram_kbytes` — RAM resource allocated for the transaction.
    * `(integer) max_storage_kbytes` — Storage resource allocated for the transaction.
    * `(integer) delay_sec` — Delay time (in seconds).
    * `(array of objects) actions` — Actions composed of transaction.
    * `(array of integers/strings) transaction_extensions` — Objects included in the transaction.
  * `(string) available_keys` — Provide the available keys.

**Request example:**  
```
curl --request POST  -d '{"transaction" : {"trx_id":"d48b...90ee8","sender":"","sender_id": "308457...031097", "payer":"cyber.token", "delay_until":"2019-03-29T05:42:03.000", "expiration":"2019-03-29T05:52:03.000", "published":"2019-03-29T05:32:03.000", "transaction":"eead9d5...59530000000000"}, "available_keys" : ["GLS62zK8V5V...YUHM3YeLZg"] }' <node>/v1/chain/get_required_keys
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  null // String containing keys. Empty string means the transaction has already been signed
}
```

## get_transaction_id
The request returns ID of a transaction using specified parameters if such transaction is in chain DB, otherwise returns a message like this one "Unknown Transaction ID: ...".  

**Params:**  
Optional set params is used.  

**Request examples:**  
```
curl --request POST  -d '{"context_free_actions":[],"actions":[{"account":"cyber.token","name":"create","authorization":[{"actor":"cyber.token","permission":"active"}],"data":{"issuer":"cyber","maximum_supply":{"amount":1000000000000,"decs":4,"sym":"SYS"}},"hex_data":"0000000080ab8e470010a5d4e80000000453595300000000"}],"transaction_extensions":[]}' <node>/v1/chain/get_transaction_id
```
```
curl --request POST  -d '{"context_free_actions":[],"actions":[{"account":"cyber.token","name":"create","authorization":[{"actor":"cyber.token","permission":"active"}],"data":"0000000080ab8e470010a5d4e80000000453595300000000"}],"transaction_extensions":[]}' <node>/v1/chain/get_transaction_id
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "trx_id": "d48b5b27f90...a6f85fc90ee8"
}
```

## get_agent_public_key
The request returns a key needed to sign a transaction if such key is in chain DB, otherwise returns an empty string.  

**Params:**
  * `(account_name) account` — An agent for which the key is requested. An agent is a potentially active account who has the staked tokens.
  * `(symbol) symbol` — Identifier of a staked token, that is a token cost accuracy in the form of decimal places number and a token name, consisting of a set of capital letters.

**Request example:**  
```
curl --request POST  -d '{"account" : "alice", "symbol" : "4,CYBER"}' <node>/v1/chain/get_agent_public_key
```

### Responses
**Code:** 200 OK  

**Value:**
```
{
  "signing_key": "GLS62zK8V5U4fsg...Wn"
}
```

## resolve_names
The request returns the resolved domain and user names retrieved from the JSON format array. The request does not successfully complete if at least one name from the array is invalid.  

**Params:**
  * Kinds of supported names:
    * `(string) domain` — A domain name.
    * `(string) @domain` — A domain name.
    * `(string) username@domain` — A username is in a domain scope.
    * `(string) username@@domain` — A username is directly in a domain scope.

**Request example:**  
```
curl --request POST --data '["@alice","alice@golos","bob@@gls"]'  <node>/v1/chain/resolve_names
```

### Responses
**Code:** 200 OK  

**Value:**
```
[
  {"resolved_domain":"rhdaax5zvnd"},                            // "alice" was resolved to "rhdaax5zvnd"
  {"resolved_domain":"gls","resolved_username":"rhdaax5zvnd"},  // "golos" was resolved to "gls"
  {"resolved_username":"ertojevqcywn"}                          // "bob" was resolved to "ertojevqcywn"
]
```

## get_proxy_status
The request retrieves an information for specified proxy account.  

**Params:**
  * `(account_name) account` — A proxy account name.
  * `(symbol) symbol`  — Identifier of a token (provided by grantors), that is a token cost accuracy in the form of decimal places number and a token name, consisting of a set of capital letters.

**Request example:**  
```
curl --request POST  -d '{"account" : "alice", "symbol" : "4,CYBER"}' <node>/v1/chain/get_proxy_status
```


### Responses
**Code:** 200 OK  

**Value:**
```
{
  "account_name": "alice",
  "symbol": [         // Type of tokens provided to the proxy account by grantors for voting
    {
      "accuracy": "4",
      "symbol_code": "CYBER",
    }
  ]
  "proxylevel": 1,
  "proxies_count": 3  // Number of grantors
}
```