# Section_4 Connecting to a node

Connection and work with the blockchain is performed using the utility `cleos`. Before executing this instruction, make sure that the log file `nodeosd` stops the flow of new information and that its text does not contain error messages. The `cleos` utility requires for its work a running service for saving private keys `keosd`. The `keosd` service runs on the user's computer. One of the options to launch `keosd/cleos` is to launch them in the form of a Docker container. For this you need:  

**4.1 Start the `keosd` service**  
Start the `keosd` service and connect it to the Docker network where `nodeos` is running:
```
sudo docker run -ti -d --name keosd --net cyberway-net cyberway/cyberway:stable /opt/cyberway/bin/keosd
```

**4.2 Start the `cleos` service**  
Assign alias to run `cleos` in the container` keosd`:
```
alias cleos='sudo docker exec -ti keosd cleos --url http://nodeosd:8888 '
``` 

**4.3 Check connection to blockchain**  
Check successful connection to blockchain. To do this, execute the command:
```
cleos get info
```
The connection is considered successful if no errors occurred during the execution of the command.  
 
**4.4 Create storage**  
Create storage for private keys:
```
cleos wallet create --file wallet.pass
```
In case of stopping the use of storage, the service `keosd` automatically blocks it. After this, the vault can be unlocked using the command:
```
cleos wallet unlock --password 'sudo docker exec -ti keosd cat wallet.pass'
```

**4.5 Import the private key**  
Import the private key with the command:
```
cleos wallet import --private-key <private-key>
```

