# How To Obtain A Snapshot And Deploy A Node Using The Resulting Snapshot

### Goal
Deploy a node using a system state snapshot.

### Steps

Obtain snapshot via API request:
```sh
$ curl --request POST --url http://<node>/v1/producer/create_snapshot
```
Example:
```sh
$ curl --request POST --url http://127.0.0.1:8888/v1/producer/create_snapshot
```

In successful, a file is generated containing the system state snapshot and indicating its location on the server.  
*Output*
```sh
snaphot-<hash>.bin
```

In addition to this snapshot file, two more blocklog files will be generated. These are `blocks.index` and` blocks.log`, which are also necessary to restore the system on your node.  

To deploy node using the system state snapshot, you need to specify the blocklog and path to the snapshot in command line:
```sh
$ nodeos --snapshot <snapshot-directory>/snapshot-<hash>.bin
```
**Important**  
> `--snapshot` is incompatible with `--geneis-json` and `--genesis` options as the snapshot contains genesis information.
