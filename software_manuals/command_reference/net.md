# Net

**Description**  
The subcommands can be used to interact with local p2p network connections.  

**Subcommands**
 * [Connect](#connect) — Start a new connection to a peer.
 * [Disconnect](#disconnect) — Close an existing connection.
 * [Status](#status) — Status of existing connection.
 * [Peers](#peers) — Status of all existing peers.

*****
## Connect

### Description
Start a new connection to a peer.

### Positional Parameters
 * `(string) host`— The hostname:port to connect to (required).

### Options
No options required for this subcommand.

### Command
```sh
$ cleos net connect <host>
```

### Examples
Connect to 'hostname:port'.
```sh
$ cleos net connect hostname:port
```

## Disconnect

### Description
Close an existing connection.

### Positional Parameters
 * `(string) host`— The hostname:port to disconnect from (required).

### Options
No options required for this subcommand.

### Command
```sh
$ cleos net disconnect <host>
```

### Examples
Disconnect from 'hostname:port'.
```sh
$ cleos net disconnect hostname:port
```

## Status

### Description
Status of existing connection.

### Positional Parameters
 * `(string) host`— The hostname:port to query status of connection (required).

### Options
No options required for this subcommand.

### Command
```sh
$ cleos net status <host> 
```

### Examples
```sh
$ cleos net status hostname:port
```

Given, a valid, existing 'hostname:port' parameter the above command returns a JSON response looking similar to the one below:
```
{
  "peer": "hostname:port",
  "connecting": false/true,
  "syncing": false/true,
  "last_handshake": {       // Structure explaining in detail in the Network Peer Protocol documentation section
    ...
  }
}
```

## Peers

### Description
Status of all existing peers.

### Positional Parameters
No parameters required fot this subcommand.

### Options
No options required for this subcommand.

### Command
```sh
$ cleos net peers
```