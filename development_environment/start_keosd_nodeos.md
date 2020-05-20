# 4: Start keosd and nodeosd

This section provides guidance on how to start `keosd` and `nodeosd` services.

## Step 1: Start nodeosd

### Start a node

To start a node, it is necessary to start two services — `nodeosd` and `mongo`. To simplify the startup process, use the `docker-compose` utility. Enter the directory where `docker-compose.yml` is located, and execute the services load command:
```sh
$ sudo docker-compose up -d
```

The option `-d` is required to run container in background.

### Check a launch of containers
To check if containers have been started successfully, run the following command:
```sh
$ sudo docker ps
```

To see text of log files, you can use the following commands:
```sh
$ sudo docker logs --tail 100 -f nodeosd
$ sudo docker logs --tail 100 -f mongo
```
Options:
`--tail` — sets a number of text lines;  
`-f` — indicates that it is necessary to monitor the update log file.

Check that the text of log files does not contain error messages. There should also be messages about created containers with the names `nodeosd` and `mongo`. The text should have the following form:
```sh
info  2019-03-07T06:57:09.024 thread-0  producer_plugin.cpp:1491      produce_block        ] Produced block 00000c992d36ab56... #3225 @ 2019-03-07T06:57:09.000 signed by producera [trxs: 0, lib: 2564, confirmed: 0]
```
It should also have information about blocks received over the network:
```sh
info  2019-03-07T06:57:00.096 thread-0  producer_plugin.cpp:344       on_incoming_block    ] Received block 6d6ac52bfe754174... #3222 @ 2019-03-07T06:57:00.000 signed by cyber [trxs: 0, lib: 2562, conf: 0, latency: 96 ms]
```

## Step 2: Start keosd

### Start the keosd service

Connecting a node to blockchain is done using the `cleos` utility. This utility requires a running `keosd` service to store private keys. The `keosd` service can be started on user's computer via Docker.  

Start the `keosd` service and connect it to the Docker network where `nodeosd` is running:
```sh
$ sudo docker run -ti -d --name keosd --net cyberway-net cyberway/cyberway:stable /opt/cyberway/bin/keosd
```

### Start the cleos service
Assign alias to run `cleos` in the container `keosd`:
```sh
$ alias cleos='sudo docker exec -ti keosd cleos --url http://nodeosd:8888'
```

### Check connection to blockchain
Check if your node has been connected to blockchain:
```sh
$ cleos get info
```
No errors should be while the services are running.

### Create storage for private key

```sh
$ cleos wallet create --file wallet.pass
```

The `keosd` service automatically locks storage if it is not in use. Storage can be unlocked using the command:
```sh
$ cleos wallet unlock --password 'sudo docker exec -ti keosd cat wallet.pass'
```

### Import the private key
To import your private key, you can use:
```sh
$ cleos wallet import --private-key <private-key>
```

## Troubleshutting
In case of errors while container is running, it is recommended to stop the services, remove Docker volume and create it again.
To stop the services, you can use:
```sh
$ sudo docker-compose down
```

To remove Docker volume, you can use the following command:
```sh
$ sudo docker volume rm cyberway-mongodb-data cyberway-nodeos-data
```

