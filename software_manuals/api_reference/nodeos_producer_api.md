# Producer API

The `producer_api_plugin` exposes a number of endpoints for the `producer_plugin` to the RPC API interface managed by the `http_plugin`.

For API request, it needs to perform a `POST` line with required parameters. The manual presents examples of requests using `curl`.
Return code *201* indicates successful operation.

**Producer API requests supported:**
  * [pause](#pause)
  * [resume](#resume)
  * [paused](#paused)
  * [get_runtime_options](#get_runtime_options)
  * [update_runtime_options](#update_runtime_options)


## pause
The request puts a producer node in pause state and returns nothing.  

**Params:**  
No params required.  

**Request example:**
```sh
curl --request POST  --data '' http://<node>/v1/producer/pause
```

### Responses
**Code:** 201 OK  

**Value:**  
Returns nothing.

## resume
The request switches a producer node from "pause" state to "resume" state and returns nothing.  

**Params:**  
No params required.  

**Request example:**  
```sh
curl --request POST  --data '' http://<node>/v1/producer/resume
```

### Responses
**Code:** 201 OK  

**Value:**  
Returns nothing.

## paused
The request retreives paused status for producer node.  

**Params:**  
No params required.  

**Request example:**  
```sh
curl --request POST  --data '' http://<node>/v1/producer/paused
```

### Responses
**Code:** 201 OK  

**Value:**
```
{
  "pause_production": true  // "true" if producer node is paused, "false" otherwise (that is, the node produces blocks)
}
```

## get_runtime_options
The request retreives run time options for producer node.  

**Params:**  
No params required.  

**Request example:**  
```sh
curl --request POST --data '' http://<node>/v1/producer/get_runtime_options
```

### Responses
**Code:** 201 OK  

**Value:**
```
{
  "max_transaction_time": 800,       // Time (in milliseconds) allocated to a transaction
  "max_irreversible_block_age": -1,  // Time (in seconds) allocated to irreversible block age
  "produce_time_offset_us": 0,       // Produce time offset (in microseconds)
  "last_block_time_offset_us": 0,    // Last block time offset (in microseconds)
  "max_scheduled_transaction_time_per_block_ms": 2000  // Max scheduled transaction time per block (in milliseconds)
}
```

## update_runtime_options
The operation updates run time options for producer node.  
Each of parameters specified in the operation is optional.  

**Params:**
  * `(integer) max_transaction_time` — Limits the maximum time (in milliseconds) that is allowed a pushed transaction's code to execute before being considered invalid. Defauts to *1000*.
  * `(integer) max_irreversible_block_age` — Limits the maximum age (in seconds) of the DPOS Irreversible Block for a chain this node will produce blocks on (use negative value to indicate unlimited). Defaults to *-1*.
  * `(integer) produce_time_offset_us` — Offset of non last block producing time (in microseconds). Negative number results in blocks to go out sooner, and positive number results in blocks to go out later. Defaults to *0*.
  * `(integer) last_block_time_offset_us` — Offset of last block producing time (in microseconds). Negative number results in blocks to go out sooner, and positive number results in blocks to go out later. Defaults to *0*.
  * `(integer) max_scheduled_transaction_time_per_block_ms` — Maximum wall-clock time (in milliseconds) spent retiring scheduled transactions in any block before returning to normal transaction processing. Defaults to *1000*.
  * `(integer) incoming_defer_ratio` — Ratio between incoming transations and deferred transactions when both are exhausted. Defaults to *1.0* that  means *1:1*.

**Request examples:**  
```sh
curl --request POST  -d '{"max_transaction_time": 1500, "last_block_time_offset_us": 5}' http://<node>/v1/producer/update_runtime_options
```

```sh
curl --request POST  -d '{"max_transaction_time": 1500, "max_irreversible_block_age": 5, "produce_time_offset_us": 10, "last_block_time_offset_us": 10, "max_scheduled_transaction_time_per_block_ms": 1000, "incoming_defer_ratio": 1.2}' http://<node>/v1/producer/update_runtime_options
```

### Responses
**Code:** 201 OK  

**Value:**  
Returns Nothing.

