# Net API

For API request, it needs to perform a `POST` line with required parameters. The manual presents examples of requests using `curl`.
Return code *201* indicates successful operation.

**Producer API requests supported:**
  * [connect](#connect)
  * [disconnect](#disconnect)
  * [status](#status)
  * [connections](#connections)


## connect
The request initiates a connection to a specified peer.  

**Params:**
  * `(string) endpoint` — The endpoint to connect to expressed as either IP address or URL.

**Request examples:**  
```
curl --request POST  --data '{"endpoint": "string"}' http://<node>/v1/net/connect
```

### Responses
**Code:** 201 OK  

**Value:**  
```
{
  "added connection"    // Otherwise "already connected" if the connection was already initiated before 
}
```

## disconnect
The request initiates disconnection from a specified peer.  

**Params:**
  * `(string) endpoint` — The endpoint to disconnect from, expressed as either IP address or URL.

**Request example:**  
```
curl --request POST  --data '{"endpoint": "string"}' http://<node>/v1/net/disconnect
```

### Responses
**Code:** 201 OK  

**Value:**
```
{
  "connection removed"    // Otherwise "no known connection for host" if no connection was initiated to the peer
}
```

## status
The request retreives the connection status for a specified peer.  

**Params:**
  * `(string) endpoint` — The endpoint to get the status for, to expressed as either IP address or URL.

**Request example:**  
```
curl --request POST  --data '{"endpoint": "string"}' http://<node>/v1/net/status
```

### Responses
**Code:** 201 OK  

**Value:**
```
{
  "peer": "string",          // The IP address or URL of the peer
  "connecting": true,        // "true" if the peer is connecting, otherwise "false"
  "syncing": true,           // "true" if the peer is syncing, otherwise "false"
  "last_handshake": {        // Structure holding detailed information about the connection
    "network_version": 0,    // Incremental value above a computed base. Defaults to "0"
    "chain_id": "string",    // Chain ID. Used to identify chain (sha256)
    "node_id": "string",     // Node ID. Used to identify peers and prevent self-connect (sha256)
    "key": "string",         // Authentication public key
    "time": "string",        // Date/time in the format (YYYY-MM-DDTHH:MM:SS.sss)
    "token": "string",       // Digest of time to prove we own the private key of the key above (sha256)
    "sig": "string",         // Signature for the digest
    "p2p_address": "string", // Address of the peer (IP address or URL)
    "last_irreversible_block_num": 0,        // Last irreversible block number. Defaults to "0"
    "last_irreversible_block_id": "string",  // Last irreversible block ID (sha256)
    "head_num": 0,           // Head number. Defaults to "0"
    "head_id": "string",     // Head ID (sha256)
    "os": "string",          // Operating system name
    "agent": "string",       // Agent name
    "generation": 0,         // Generation number
    "considers_gray":        // Considers gray. Optional field. Defaults to "false".
  }
}
```


## connections
The request returns an array of all peer connection statuses.  

**Params:**  
No params required.  

**Request example:**  
```
curl --request POST --data '' http://<node>/v1/net/connections
```

### Responses
**Code:** 201 OK  

**Value:**
```
[
  {
    "peer": "string",          // The IP address or URL of the peer
    "connecting": true,        // "true" if the peer is connecting, otherwise "false"
    "syncing": true,           // "true" if the peer is syncing, otherwise "false"
    "last_handshake": {        // Structure holding detailed information about the connection
      ...
    }
  }
  {...}
]
```
