
# APPENDIX A

## Procedure for checking the connection of node to CyberWay Mainnet

As soon as the `start_light.sh` script finishes, you have to make sure that docker-composer and services are working properly on your server. Docker-compose logs process information into its internal file. To get and analyze the logged information you can use the following commands:
```sh
sudo docker logs -f nodeosd
sudo docker logs -f mongo
```
or (to get a partial information)
```sh
sudo docker logs --tail 100 -f nodeosd
sudo docker logs --tail 100 -f mongo
```
The options:  
`--tail` — takes the last lines of text;  
`-f` — means that it is necessary to monitor the update log file.  

No error messages should be in the logged information.

### A1 Monitoring the successful installation
Make sure that the `nodeosd` and `mongo` containers are successfully installed. It is recommended to run the following command:
```sh
sudo docker ps
```
Containers are considered to be successfully installed if the following conditions are met:
  * no error messages in the log file;
  * the messages about installing containers with the names `nodeosd` and `mongo`;
  * the containers are in running state. 

### A2 Monitoring the start of node synchronization

Make sure that the node has initiated block synchronization. The node is considered to have started this process if the log information contains a piece of text like this one:
```
info  2019-10-01T00:28:39.752 nodeos    producer_plugin.cpp:339       on_incoming_block    ] Received block d7f859397b3f1220... #1214805 @ 2019-10-01T00:28:36.000 signed by kk5oxqhb2ird [trxs: 0, lib: 1214765, conf: 13, latency: 3752 ms]
info  2019-10-01T00:28:39.771 nodeos    producer_plugin.cpp:339       on_incoming_block    ] Received block 9f560026df0a3394... #1214806 @ 2019-10-01T00:28:39.000 signed by yzpsff4dxom4 [trxs: 0, lib: 1214767, conf: 20, latency: 771 ms]
info  2019-10-01T00:28:42.038 nodeos    producer_plugin.cpp:339       on_incoming_block    ] Received block b2141be6f9c671a3... #1214807 @ 2019-10-01T00:28:42.000 signed by 3ukodagt5qrc [trxs: 0, lib: 1214767, conf: 18, latency: 38 ms]
```
The `start_block` field in the text indicates that the node has started processing blocks.

### A3 Monitoring the node working state
Node synchronization process can take a long time. Therefore, you should monitor this process from time to time.  

Make sure that the node does not hang. To get node status information, you can use the following command:
```
cleos get info
```
The node is considered to be in working state if the received text contains information about  processing `head_block_num`, like this one:
```
{
 "server_version": "27be611d",
 "chain_id": "591c8aa5cade588b1ce045d26e5f2a162c52486262bd2d7abcb7fa18247e17ec",
 "head_block_num": 2419294,
 "last_irreversible_block_num": 2419247,
 "last_irreversible_block_id": "0024ea2f63a1e7411b75362def0befaf4698235b17956f3d4ec000491f37d5f8",
 "head_block_id": "0024ea5e6223f4c8c5fadd87e0cb0e5df04cda7aeb9b8c8c9111b56a0e19e665",
 "head_block_time": "2019-11-14T08:17:45.000",
 "head_block_producer": "1avnch3wug2o",
 "virtual_block_cpu_limit": 1400000000,
 "virtual_block_net_limit": 1048576000,
 "block_cpu_limit": 1399900,
 "block_net_limit": 1048576,
 "server_version_string": "v2.0.2-37-g27be611d0"
}
```
The `head_block_num` field indicates a current block being processed by the node.

### A4 Monitoring the completion of node synchronization

The node is considered to be successfully synchronized if it began to receive blocks. The log file should contain a piece of text like this one:
```
info  2019-10-01T00:28:39.752 nodeos    producer_plugin.cpp:339       on_incoming_block    ] Received block d7f859397b3f1220... #1214805 @ 2019-10-01T00:28:36.000 signed by kk5oxqhb2ird [trxs: 0, lib: 1214765, conf: 13, latency: 3752 ms]
info  2019-10-01T00:28:39.771 nodeos    producer_plugin.cpp:339       on_incoming_block    ] Received block 9f560026df0a3394... #1214806 @ 2019-10-01T00:28:39.000 signed by yzpsff4dxom4 [trxs: 0, lib: 1214767, conf: 20, latency: 771 ms]
info  2019-10-01T00:28:42.038 nodeos    producer_plugin.cpp:339       on_incoming_block    ] Received block b2141be6f9c671a3... #1214807 @ 2019-10-01T00:28:42.000 signed by 3ukodagt5qrc [trxs: 0, lib: 1214767, conf: 18, latency: 38 ms]
```
The `on_incoming_block` field indicates an incoming block being received by the node.
Also, make sure that the log file contains information about  the `latency` parameter with time less than 3000 ms.  

### Conclusion
The node is considered to be successfully synchronized, if the log file contains information similar to the one in this appendix.
