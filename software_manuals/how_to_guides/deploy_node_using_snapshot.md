# How To Obtain A Snapshot And Deploy A Node Using The Resulting Snapshot

### Goal
Deploy a node using its node state snapshot.

### Steps

Obtain snapshot via API request:
```sh
$ curl --request POST --url http://<node>/v1/producer/create_snapshot
```
Example:
```sh
$ curl --request POST --url http://127.0.0.1:8888/v1/producer/create_snapshot
```

*Output*
```sh
snaphot-<hash>.bin
```

The file generated is the node state snapshot. This file alone will not be enough to restore your node. You will also need two more blocklog files. These are `blocks.index` and` blocks.log`. Before restoring a node, these files must be in the same place where they were generated.  

To deploy node using the node state snapshot, you need to specify path to the snapshot in command line:
```sh
$ nodeos --snapshot <snapshot-directory>/snapshot-<hash>.bin
```
**Important**  
> `--snapshot` is incompatible with `--geneis-json` and `--genesis` options as the snapshot contains genesis information.
