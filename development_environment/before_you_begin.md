# 1: Before You Begin

We appreciate your interest in contributing to the CyberWay platform! We always welcome contributions from our community to make our code and docs better.  

This section provides guidance on how to install the CyberWay binaries on your server. To do this, you need to:
 - Prepare server
 - Clone CyberWay repository
 - Run the start_light.sh script.

## Step 1: Prepare server

The server where CyberWay binaries are installed, must meet the following characteristics (or above):
 - RAM size (a min. of): 8 GB
 - disk space (a min. of): 20 GB
 
One of the following operating systems must also be installed on the server:
 - Ubuntu 16.04
 - Ubuntu 18.04
 - MacOS Darwin 10.12 (or later versions)
 - Centos 7
 - Fedora 25 (or later versions)
 - Mint 18

**WARNING !!!** If you have a private key,iIt is strongly recommended to **save your private key** before proceeding.  

If you have previous version of CyberWay installed on your system, please uninstall it before proceeding.

## Step 2: Clone CyberWay repository
Create a directory where you will upload sources and clone CyberWay repository:
```sh
mkdir ~/cyberway.launch
git clone https://github.com/cyberway/cyberway.launch  ~/cyberway.launch
```

## Step 3: Run the shell-script:
Go to the created CyberWay space and run the `start_light.sh` script:  (!!! start_testnet.sh !!!)
```sh
cd ~/cyberway.launch
 ./start_light.sh
```

Configuration file, genesis data (a snapshot of CyberWay system state) and docker file should appear on your server:
 - /etc/cyberway/config.ini
 - /var/lib/cyberway/genesis-data/genesis.json
 - /var/lib/cyberway/genesis-data/genesis.dat
 - /var/lib/cyberway/docker-compose.yml

During the script execution, the following operations are done:
 - creating the environment for CyberWay configuration on the server;
 - downloading the Docker image;
 - downloading the CyberWay genesis, including the files `genesis.json` and `genesis.dat`;
 - creating Docker volumes to store the system state database and chain data;
 - start the services `nodeosd` and `mongo`. Setting up these services occurs by `docker-compose.yml`.

The script runs the current version of CyberWay with the genesis data uploaded which is used as input. Successful completion of the script means a successful server connection to Mainnet as a seed-node.  

The procedure for checking a connection of your server to Mainnet as a seed-node is given [here](https://docs.cyberway.io/validators/mainnet_connection/appendix_a).  

If you followed all the instructions given in that appendix A and made sure that your server is fully synchronized with Mainnet, that meant that the server connected to Mainnet as a seed-node.

