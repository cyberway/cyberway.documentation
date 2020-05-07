# Convert

**Descriptions**  
The subcommands are required to pack and unpack transactions as well as to convert data from JSON format to digital code and vice versa  

**Subcommands**
 * [Pack Action Data](#Pack-Action-Data) — From JSON action data to packed form.
 * [Pack Transaction](#Pack-Transaction) — From plain signed JSON to packed form.
 * [Unpack Action Data](#Unpack-Action-Data) — From packed to JSON action data form.
 * [Unpack Transaction](#Unpack-Transaction) — From packed to plain signed JSON form.

*****
# Pack Action Data

### Description
The subcommand converts action data from JSON format to packed form.

### Positional Parameters
 * `(string) account` — The name of the account that hosts the contract.
 * `(string) name` — The name of the function that's called by this action.
 * `(string) unpacked_action_data` — The action data expressed as JSON.

### Options
No options required for this subcommand.

### Examples
```
$ cleos convert pack_action_data c.gallery unlinkauth '{"account":"test1", "code":"test2", "type":"cybertype"}'
```
```
000000003500b1be00000000008fa1ca0000a47deaea2903
```

# Pack Transaction

### Description
The subcommand converts a transaction from plain signed JSON to packed form.

### Positional Parameters
 * `(string) transaction` — The plain signed JSON.

### Options
 * `--pack-action-data` — Pack all action data within transaction, needs interaction with `nodeos`.

### Examples
```
$ cleos convert pack_transaction '{
  "expiration": "2020-02-02T18:01:32",
  "ref_block_num": 22654,
  "ref_block_prefix": 4107128227,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "max_ram_kbytes": 0,
  "max_storage_kbytes": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "c.gallery",
      "name": "create",
      "authorization": [{
          "actor": "cmnwrtlcdzcl",
          "permission": "active"
        }
      ],
      "data": "0000000000fe...3d8ab000000"
    }
  ],
  "transaction_extensions": []
}'
```
```
{
  "signatures": [],
  "compression": "none",
  "packed_context_free_data": "",
  "packed_trx": "7429ab7f379...00000"
}
```


# Unpack Action Data

### Description
The subcommand converts action data from packed to JSON format.

### Positional Parameters
 * `(string) account` — The name of the account that hosts the contract.
 * `(string) name` — The name of the function that's called by this action.
 * `(string) packed_action_data` — The action data expressed as packed hex string.

### Options
 * `--json`, `-j` — Output in JSON format.

### Examples
```
$ cleos convert unpack_action_data c.gallery unlinkauth 000000003500b1be00000000008fa1ca0000a47deaea2903
```
```
{
  "account": "test1",
  "code": "test2",
  "type": "cybertype"
}
```


# Unpack Transaction

### Description
The subcommand converts a transaction from packed to plain signed JSON form.

### Positional Parameters
 * `(string) transaction` — The packed transaction JSON (string containing packed_trx and optionally compression fields).


### Options
 * `--unpack-action-data` — Unpack all action data within transaction, needs interaction with `nodeos`.

### Examples
```
$ cleos convert unpack_transaction '{
  "signatures": [
    "SIG_K1_K6gS7...YE596eh"
  ],
  "compression": "none",
  "packed_context_free_data": "",
  "packed_trx": "7429ab7f379...00000"
}'
```
```
{
  "expiration": "2020-02-02T18:01:32",
  "ref_block_num": 22654,
  "ref_block_prefix": 4107128227,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "max_ram_kbytes": 0,
  "max_storage_kbytes": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "c.gallery",
      "name": "create",
      "authorization": [{
          "actor": "cmnwrtlcdzcl",
          "permission": "active"
        }
      ],
      "data": "0000000000fe...3d8ab000000"
    }
  ],
  "transaction_extensions": [],
  "signatures": [
    "SIG_K1_K6gS7...YE596eh"
  ],
  "context_free_data": []
}
```

