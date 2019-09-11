# Section_3 Create Container

The process of building services is building the `Docker volume` volumes:    

**3.1 Create Docker volumes**  
 Create Docker volumes to store the system state database and chain data by executing:
```
sudo docker volume create cyberway-mongodb-data
sudo docker volume create cyberway-nodeos-data
```
**3.2 Perform a volume creation check**  
To do this, execute:
```
sudo docker volume ls
```
Creating volumes is considered successful if the issue of the command contains information about the volumes created:
```
local cyberway-mongodb-data
local cyberway-nodeos-data
```
**3.3 Start the services**  
 To start a node, it is necessary to start two services — `nodeosd` and `mongo`. To simplify the startup process, use the `docker-compose` utility.
Enter the `~/testnet` directory (where `docker-compose.yml` is located), and execute the services load command:
```
sudo docker-compose up -d
``` 
The option `-d` is used to run the container in the background.  

**3.4 Check the launch of containers**  
To check the successful launch of containers, the following cmmand should be executed:
```
sudo docker ps
```
Creating containers is considered successful if there are no error messages in the text of the log files and there are messages about creating containers with the names `nodeosd` and `mongo`. To analyze the text of log files, you can use the following commands:
```
sudo docker logs --tail 100 -f nodeosd
sudo docker logs --tail 100 -f mongo
```
The options:  
`--tail` — sets the number of last lines of text;  
`-f` — indicates that it is necessary to monitor the update log file.  

The text of the log file `nodeosd` should also contain information about the generated blocks, as well as about the blocks received from Testnet. The information in the log file about the generated block should have the following form:
```
info  2019-03-07T06:57:09.024 thread-0  producer_plugin.cpp:1491      produce_block        ] Produced block 00000c992d36ab56... #3225 @ 2019-03-07T06:57:09.000 signed by producera [trxs: 0, lib: 2564, confirmed: 0]
```
The information in the log file about the blocks received over the network should have the following form:
```
info  2019-03-07T06:57:00.096 thread-0  producer_plugin.cpp:344       on_incoming_block    ] Received block 6d6ac52bfe754174... #3222 @ 2019-03-07T06:57:00.000 signed by cyber [trxs: 0, lib: 2562, conf: 0, latency: 96 ms]
```
If the testnet is successfully launched, information about blocks received over the network is periodically saved to the log file.  

**3.5 Recommendation**  
> In case of errors during container launch, it is recommended to stop the functioning of the services, remove `Docker volume` and create it again.  

To stop the functioning of services execute:
```
sudo docker-compose down
```
To remove `Docker volume`, use the following command:
```
sudo docker volume rm cyberway-mongodb-data cyberway-nodeos-data
```
To re-create `Docker volume`, you need to re-execute the instructions, starting with p. 3.1. In case of errors when re-creating `Docker volume`, you should report this to the CyberWay development team.
 
