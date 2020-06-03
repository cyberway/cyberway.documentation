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

The node will stop receiving data during processing of the request. If successful, a message will be displayed indicating the system state snapshot location on server. Message type:
```sh
snaphot-<hash>.bin
```

In addition to this snapshot, you also need the blocklog files. These are `blocks.index` and `blocks.log`.

To deploy a node using a system state snapshot, you need to specify the blocklog and path to the snapshot in command line:
```sh
$ nodeos --snapshot <snapshot-directory>/snapshot-<hash>.bin
```
**Important**  
> `--snapshot` is incompatible with `--geneis-json` and `--genesis` options as the snapshot contains genesis information.
