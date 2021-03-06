# Golos blockchain transit (migration)

## A witness’ guide

This guide provides generic, practical advice for witnesses of Golos blockchain for migration from Golos blockchain to Golos application running on top of CyberWay. The manual contains information about transit procedure as well as actions necessary to be taken by Golos witnesses, which ensure voting nd launch of CyberWay with the data taken from Golos blockchain.

## Description of the Golos blockchain transit procedure

The transit procedure starts immediately upon the completion of 0.21.0 Hardfork and includes the following steps:  
   * Voting for transit by Golos witnesses
   * Snapshot dump of Golos system state from witnesses’ nodes.
   * Golos chain work termination.
   * Genesis file generation.
   * CyberWay launch with data from Golos chain.
   * Installation and deployment of Golos application.  

The fundamental condition for successful completion of the transit is that Golos application is installed and successfully deployed in CyberWay environment.  

### Witnesses’ Voting for Transit 

After 0.21.0 Hardfork acceptance and by the moment of the actual beginning of transit (August 15th 2019 at 12 noon Moscow time), Golos witnesses will get access to the voting operation. Each Golos witness has to make a decision concerning the transit and send a transaction signed with his/her own key.  

Consensus will be considered achieved if the number of witnesses who have agreed to transit is at least 16 out of the top 19 witnesses. As soon as consent transaction is received from the 16th witness and put into the last irreversible block (abbr., LIB), the voting process will be completed and the transaction containing the voting results will be sent to the block log.  

At that instant the witnesses voting procedure for transiting Golos to CyberWay blockchain will be considered successfully completed.  


### Golos State Snapshot 


If 16 Golos witnesses vote positively for transit, the script https://github.com/cyberway/golos.transit/blob/master/transit.sh will begin to run. All further transit procedures will be performed automatically.  

The procedure for taking a dump snapshot of Golos system state will be launched. Data files will be created to save a fixed state of Golos blockchain (system state) on those nodes in which the directories for uploading system state data are set in the configuration file.

### Golos Termination 
 
Golos network will continue to produce empty blocks for the time set in the settings straight after receiving the transaction with the final consent of the 16th witness’, so that this transaction would fall into the LIB block on other witnesses’ nodes. However, during this cycle, the network will not process transactions and, consequently, operations such as publishing posts and comments, transferring funds, putting “likes”, etc. will not be performed. Emission and token transfer operations will also be stopped. This allows to register LIB block with consent to transit from the 16th witness on all nodes of the Golos blockchain.
  
 
### Genesis File Generation

`Сreate genesis` utility generates a genesis file - the initial data for launching CyberWay - with the data taken during snapshot from Golos blockchain. This utility transfers all the information necessary to restart a paused chain on CyberWay and deploy Golos application. `Create genesis` utility will deliver the data from all the accounts, balances, open posts, etc. (i.e. Golos system data) to the new environment.  

The genesis generated by Golos Core team will be available via the link specified in the `genesis_data.link` file in [cyberway.launch github](http://github.com/cyberway/cyberway.launch) repository. `Genesis.info` file will be placed in the very same repository with a full description of the genesis parameters.  

### CyberWay Launch

CyberWay chain will be launched on the witnesses’ nodes which voted for the transit and, respectively, became validators on CyberWay. Before starting the chain the nodes must form a network. The connection will occur through seed nodes. The file with the seed nodes ip-addresses will be saved in [cyberway.launch github](http://github.com/cyberway/cyberway.launch) repository. As soon as the number of nodes connected by ip-address reaches the value specified in the genesis parameter CyberWay chain will be launched with all Golos blockchain data and the production of blocks will begin.  

`Emit` pending transaction will be executed one hour after starting CyberWay, which will initiate the procedure of closing the posts.  

## CyberWay Validator Node Requirements

The following hardware characteristics must be met for future CyberWay nodes:  
   * Disk space amount - (at least) 80 GB;
   * RAM memory - (at least) 16 GB.  

The nodes must have one of the following operating systems installed:  
   * Ubuntu (versions recommended: 16.04 or 18.04);
   * MacOS Darwin 10.12 (or later);
   * Centos 7;
   * Fedora 25 (or advanced);
   * Mint 18.  

The following software must be installed on the nodes:
   * docker;
   * docker-compose.  
 

## Sequence of Actions to be Taken by Witnesses

### Installing the HF-0.21.0 Version to the Node

The installation procedure on the HF-0.21.0 node is similar to the installation procedure of previous versions and can be performed using the [HF-18 Installation Guide](https://github.com/GolosChain/golos/wiki/Build-instruction).  

Attention! A replay of the chain is required as a must upon the completion of HF-0.21.0 installation.  

> ** Note: **  
> Witnesses not willing to support the transit decision made at the [referendum](https://steemit.com/golos/@golos/referendum-results) must not take any action. ** Non-participation is interpreted as voting “AGAINST.” **  
> Witnesses approving the decision of the [referendum](https://steemit.com/golos/@golos/referendum-results) must vote. ** Participation is interpreted as voting "FOR". **


### Voting for Transit

*Attention! By following the instructions in this paragraph you vote for the transit and initiate the transit procedure automatically!*  

Access to `cyberway.launch` repository will open upon completion of the installation of HF-0.21.0 and the beginning of transit (12-00 Moscow time, August 15, 2019). Both auxiliary scripts and CyberWay launch data will be placed in this repository.  

### Witnesses must initiate the transit procedure in any preferable way

**Option_1**  

Witnesses willing to complete the transit procedure using the genesis data generated directly on their node must:
  * upload the contents of the `cyberway.launch` repository to the node using the command
```
       git clone https://github.com/cyberway/cyberway.launch.git
```
  * provide the following conditions:  
    * Golos application must be installed in the docker container and the image name must remain as `golos-default`;
    * The configuration file should be moved to /etc/golosd/config.ini;
    * The contents of Golos application directory should be moved to /var/lib/golosd.
  * run the script `start_check_state.sh`.  
 
The script mentioned above initiates the following processes:  
  * Voting procedure;
  * Waiting for network termination;
  * Formation of genesis procedure;
  * CyberWay launch with the genesis data.  

The option_1 transit procedure will be considered successfully completed if and only if:
  * CyberWay environment has been installed in the docker container;
  * /etc/cyberway/config.ini configuration file has been created;
  * /var/lib/cyberway directory for storing data has been created;
  * All the tcp-ports necessary are being ported.  

> ** Note: **  
> Transit wise it’s necessary that the node characteristics must comply with the requirements given in the subsection [Requirements for validator nodes](#cyberway-validator-node-requirements) .  

**Option_2** (recommended)  

Witnesses willing to complete the transit procedure using the genesis data generated by Golos Core team (i.e. with the minimum number of actions required):  

  1) upload the contents of the `cyberway.launch` repository to the node using the command
```
        git clone https://github.com/cyberway/cyberway.launch.git
```

  2) vote either with `cli_wallet`
```
        cli_wallet transit_to_cyberway <witness’ account> true
```

or using a script call  
```
         sudo ./transit.sh transit-approve
```
 3) run the `start_light.sh` script  

The option_2 transit procedure will be considered successfully completed if and only if:  
  * CyberWay environment has been installed in the docker container;
  * /etc/cyberway/config.ini configuration file has been created;
  * /var/lib/cyberway directory for storing data has been created;
  * All the tcp-ports necessary are being ported.  

Witnesses that install CyberWay on a separate server must perform the following steps  
  * both 1) and 2) on the old node  
and  
  * both 1) and 3) on the server where CyberWay is installed.  


## Sequence of actions to be taken by users who are not among the former Golos witnesses but are willing to become CyberWay validators 

Users inclined to install CyberWay on their server and join the CyberWay blockchain must run the `start_light.sh` script taken from `cyberway.launch repository`.

> ** Note: **  
> Transitwise it’s necessary that the node characteristics must comply with the requirements given in the subsection [Requirements for validator nodes](#cyberway-validator-node-requirements) 



## Communication

Communication at the time of the transit will be conducted via two mediums:  

  * [CyberWay_Validators](https://t.me/cyberway_validators) - a chat for feedback and information exchange between our dev team and the validators
  * [CyberWay Launch Updates](https://t.me/joinchat/AAAAAFBrnD_sLTXlWw3lJg) - a channel to coordinate actions at the command execution level.

*****  
