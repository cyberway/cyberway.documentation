# Section_5 List of commands applicable to any kind of container

Below is a list of commands that can be used to work with any kind of container.  

**5.1 Start the container**  
Establishing a connection with `docker` and running the container:  
```
sudo docker exec -ti nodeosd /bin/bash
```

**5.2 Getting the log file**  
Getting the text of the log file about the container:
```
sudo docker logs --tail 10 -f nodeosd
```

**5.2 Connection via cleos to the node**  
Connection via cleos to the node to verify its successful operation (to execute this instruction, the wallet wallet must be configured):
```
sudo docker exec -ti nodeosd \
    /opt/cyberway/bin/cleos
```

**5.3 Launching the containers**  
Container launch command (the command is executed from the directory where the `docker-compose.yml` file is located):  
```cpp
sudo docker start nodeosd mongo
```

**5.4 Stop the containers**  
Container stop command (the command is executed from the directory where the `docker-compose.yml` file is located):
```cpp
sudo docker stop nodeosd mongo
```
