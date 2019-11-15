
## 1 Server characteristics requirements

CyberWay software is installed on a server using the Docker platform available and the Docker-compose command-line tool. Docker platform provides the following:  
  * creation of necessary software environment, including required list of libraries, regardless of the operating system version;
  * creation of environment isolated from unnecessary temporary files stored by the system in the constructed space.  

The server should satisfy the following characteristics:  
  * RAM memory (not less): 16 GB;
  * disk space (not less): 80 GB;
  * CPU High Clock Speed 4+ Cores.  

One of the following operating systems should be installed on the server:
  * Ubuntu (version recommended: 16.04 or 18.04);
  * MacOS Darwin 10.12 (or later version);
  * Centos 7;
  * Fedora 25 (or later version);
  * Mint 18.  

Also, the following software should be installed on the server:
  * docker;
  * docker-compose.  

If there is no server with the required operating system, you can use a server with the operating system of the Linux family. It is possible to install the CyberWay software on such server using Docker platform.  
Installation of the CyberWay software on a server running under any other system is not supported.  

## 2 Getting started
You have to follow all the instructions excluding section 5 if you need to connect a server to Mainnet as a seed-node.  

You have to follow all the instructions given in this Guide if you need to register on CyberWay as a validator and connect a server to Mainnet as a validator node.  

**WARNING !!!** It is strongly recommended **to save your private key** before following the instructions given in this Guide.  

## 3 Downloading the CyberWay software to a server
Download the current version of CyberWay blockchain into a separate server space where the docker image will be placed.  
```sh
git clone https://github.com/cyberway/cyberway.launch  ~/cyberway.launch
```
Make sure that no error messages were appeared during copy repository.  

## 4 CyberWay deployment and start the containers
Go to the Cyberway space and run the shell-script:
```sh
cd ~/cyberway.launch
 ./start_light.sh
```
The script executes all the instructions required to connect the server to Mainnet in an automatic mode. During the script execution, the following operations are done: 
  * creating the environment for CyberWay configuration on the server;
  * downloading the Docker image;
  * downloading the CyberWay genesis, including the files `genesis.json` and `genesis.dat`;
  * creating Docker volumes to store the system state database and chain data;
  * start the services `nodeosd` and `mongo`. Setting up these services occurs by `docker-compose.yml`. 

The script runs the current version of CyberWay with the genesis data uploaded which is used as input. Successful completion of the script means a successful server connection to Mainnet.  

However, after the script finishes, it is recommended to check whether all operations for deploying CyberWay on the server and connecting this server to Mainnet have been successfully completed or not. All process information is logged by Docker. The procedure for checking the successful connection of the node to Mainnet is given in [Appendix A](https://cyberway.gitbook.io/en/validators/mainnet_connection/appendix_a).  

If you followed all the instructions given in appendix A and made sure that your server is fully synchronized with Mainnet, that meant that the server connected to Mainnet as a seed-node. Congratulations!!!

## 5 Registration as a validator
The following instructions are for those users who want their node to be not only connected to Mainnet, but also able to produce blocks. To do this, you can follow the section 5 instructions to get the status of a validator. No error messages should appear when the commands are executed.  

### 5.1 Deposit the minimal amount of staked tokens
The validator should have a certain amount of staked tokens on his/her balance sheet. The minimum number of staked tokens should be not less than 50000. If such an amount is not on your balance sheet, you have to get the tokens first. To deposit the minimum amount of tokens on your balance, you can use the following command:
```
cleos system regproducer <your account> < key digital signature> --min-own-stake 500000000
```
The argument `min_own_staked` is a minimum amount of CYBER tokens required to become a validator.  

### 5.2 Specify the public and private keys in configuration file
Specify your account name and both public and private keys in the configuration file `config.ini`. You can specify the keys that you received during registration, or generate new ones by executing the command:
```
cleos wallet create_key --to-console
```

Set variables in the `/etc/cyberway/config.ini` file:
```
signature-provider=<public digital signature>=KEY:<private digital signature>
producer-name=<your account>
```  
### 5.3 Restart the node
After making changes to the configuration file, you should restart the node:
```
sudo docker stop nodeos -t 120
sudo docker-compose up -d
```
The argument `-t ` sets the time required to complete all processes.

### 5.4 Block producing
A seed-node needs to get on the schedule list so it can produce blocks. The validator has to register as a block producer and gain required number of votes to be in the top-validator list. 

### 5.5 Control the creation and signing of blocks
The procedure for checking the creation and signing of blocks by your node has been connected to Mainnet is given in [Appendix B](https://cyberway.gitbook.io/en/validators/mainnet_connection/appendix_b).

If you followed all the instructions given in Appendix B and made sure that your node produces blocks signed by you, that meant that the server connected to Mainnet as a validator node and you become a validator. Congratulations!!!

## 6 Recommendation  
In case of errors when the container is running, it is recommended to stop the services, remove `Docker volume` and run `start_light.sh` again.  

To stop the functioning of services you can execute:
```sh
sudo docker-compose down
```
To remove `Docker volume`, you can use the following command:
```sh
sudo docker volume rm cyberway-mongodb-data cyberway-nodeos-data
```
## 7 Useful commands that can be applied to any kind of container

### 7.1 Connection with docker and running a container 
```sh
sudo docker exec -ti nodeosd /bin/bash
```
### 7.2 Obtaining the log file information about a container 
```sh
sudo docker logs --tail 10 -f nodeosd
```
### 7.3 Connection to a node via cleos to check its operation
```sh
sudo docker exec -ti nodeosd /opt/cyberway/bin/cleos
```
### 7.4 Start the containers 
The command is executed from the directory where docker-compose.yml is located.
```sh
sudo docker start nodeosd mongo
```
### 7.5 Stop the containers 
The command is executed from the directory where docker-compose.yml is located.
```sh
sudo docker stop nodeosd mongo
```

**Useful links:**  
https://docs.cyberway.io/validators/voting_for_validators  
https://docs.cyberway.io/devportal/create_development_wallet  
