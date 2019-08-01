
#  Section_2 Configuring the Docker Image

**2.1 Create a cyberway repository in a separate space**  
Open a command window and enter the `cyberway` directory where the Docker image will be created. Next, execute:
```
git clone https://github.com/cyberway/cyberway
```
The space from which the command was executed must copy the `cyberway` directory with its contents. Error messages should not appear during the copy process.  

**2.2 Create a separate testnet directory**  
The location of this directory is chosen arbitrarily (hereafter the `~/testnet` directory is used):
```
mkdir -p ~/testnet
```
**2.3 Copy the docker files**  
In the newly created directory `~ / testnet` copy the files `docker-compose.yml` and sample configuration file `config.ini` from the Cyberway repository. Used to copy commands:
```
cp Docker/config.ini ~/testnet/
cp Docker/docker-compose.yml ~/testnet/
```
**2.4 Go to the ~/testnet directory**  
All other commands must be executed from this directory.
```
cd ~/testnet/
```
**2.5 Configure settings in the config.ini configuration file**    

**2.5.1** Set the system status database address. The recommended address for the `chaindb_address` parameter is:
```
chaindb_address = mongodb://mongo:27017
``` 
**2.5.2** Set the addresses of the remaining nodes of the network to which you want to connect:
```
p2p-peer-address = <Node address>
```
The parameter `p2p-peer-address` is configured separately for each connected network node. To connect *N* network nodes, you need to specify this parameter *N* times. In order to start the node correctly and connect to `Testnet` (provided by the CyberWay developers), it is recommended to specify the address` 116.203.104.164:9876`.  

**2.5.3** To receive incoming connections from other network nodes, you have to uncomment the `p2p-listen-endpoint` parameter and specify the address of the network interface on which you should expect connections, as well as the port number. Setting the interface address in the form `0.0.0.0` allows the `nodeosd` service to listen to connections from other network nodes on all available network interfaces. Recommended address:
```
p2p-listen-endpoint = 0.0.0.0:9876
```
**2.5.4** If a user wants to log in as a validator, she/he needs to additionally specify the producer name and the keys that will be used to sign the blocks.  

By default, the `producer-name` parameter is commented out. In this case, the node is used only to connect to `Testnet` without producing blocks. To connect to `Testnet` with the ability to produce blocks, the validator should to additionally set the parameters `producer-name` and `signature-provider`.
```
producer-name = <Producer name>
signature-provider = <Public key>=KEY:<Private key>
```
These parameters can be set only by the validator, since it is she/he who has the data about the account name and the keys that must be used to create the blocks. The validator has to generate the values ​​of the private and public keys independently (or use the Golos blockchain keys). The private key of the validator unit is used to sign the blocks.  

The following table provides a recommended list of configuration file parameters used to connect a node to `Testnet`, as well as to configure a validator node. 

Parameter | Connecting a node to Testnet | Setting up the validator  
:-----------|:-------|:-------  
producer name | — | +  
signature provider | — | +  
chaindb_address | + | +  
p2p-peer-address | + | +  
p2p-listen-endpoint | +/— | +/—  
  

**2.6 Generate genesis data**   

**2.6.1** Download the archived file with the genesis contents to the `~/testnet` directory and unpack it by executing:  
```
wget http://download.golos.io/genesis.tar.gz
tar -zxf genesis.tar.gz
```
After these commands are executed, the `genesis` directory should appear in `~/testnet`, containing the files `genesis.dat` and `genesis.dat.map`.  

**2.6.2** Rename the `genesis` directory to `genesis-data` by executing:
```
mv genesis genesis-data
```

**2.6.3** In the `~/testnet/genesis-data` directory create a file `genesis.json` with the following contents:
```
{
  "initial_timestamp": "2019-03-01T12:00:00.000",
  "initial_key": "GLS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
  "initial_configuration": {
    "max_block_net_usage": 1048576,
    "target_block_net_usage_pct": 1000,
    "max_transaction_net_usage": 524288,
    "base_per_transaction_net_usage": 12,
    "net_usage_leeway": 500,
    "context_free_discount_net_usage_num": 20,
    "context_free_discount_net_usage_den": 100,
    "max_block_cpu_usage": 2500000,
    "target_block_cpu_usage_pct": 1000,
    "max_transaction_cpu_usage": 1800000,
    "min_transaction_cpu_usage": 100,
    "max_transaction_lifetime": 3600,
    "deferred_trx_expiration_window": 600,
    "max_transaction_delay": 3888000,
    "max_inline_action_size": 4096,
    "max_inline_action_depth": 4,
    "max_authority_depth": 6
  },
  "initial_chain_id": "0000000000000000000000000000000000000000000000000000000000000000"
}
```
