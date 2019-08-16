
## Quick Reference Guide

If you want to register on the CyberWay network as a validator, you should follow these steps.

## Machine performance requirements

First of all, you need to prepare your machine, which should satisfy the characteristics:
  * Disk space amount - (at least) 80 GB;
  * RAM memory - 8 GB (16 - recommended)
  * CPU High Clock Speed 4+ Cores  


One of the following operating systems should be installed on your machine :
  * Ubuntu (versions recommended: 16.04 or 18.04);
  * MacOS Darwin 10.12 (or later);
  * Centos 7;
  * Fedora 25 (or advanced);
  * Mint 18.


Also, the following software must be installed on the machine :
  * docker;
  * docker-compose.  

## Actions to be taken
Your docker container must be named as golos-default.  

Create a workspace and execute the commands  
```
   git clone https://github.com/cyberway/cyberway.launch
   cd cyberway.launch
   sudo ./start_light.sh
```
Your docker container must be named as golos-default. If the docker container named differently, then do the following:  
```
   nano transit.sh
```
Set the variable 
```
goloschain_name=”golos-default”
```
Check that  
  * the configuration file has been moved to `/etc/golosd/config.ini` 
  * the contents of Golos application directory has been moved to `/var/lib/golosd`.

If genesis hash falls, then run:  
```
    cd /var/lib/cyberway/genesis-data
    wget https://download.cyberway.io/genesis.dat
```

Edit the configuration file:  
```
    nano /etc/cyberway/config.ini
```  

Specify both public and private keys in the configuration file (specify golos keys, if you had them. Take the values from old golos witness_update op. Otherwise, use the keys given to you during the registration.):  
```
signature-provider=GLS7Q3iZkBe4ukfPeeW1iHwaDRu3dh5pzM9KNqDWDHzh5WR9v4wfY=KEY:5j****
producer-name=bzhnnvqtyzco
```  

Wait a full synchronization and run:  
```
   sudo dockerexec -ti nodeosd /bin/bash
   cleos wallet create --to-console
   cleos wallet import --private-key <active-key>

   cleos push action cyber.stake setminstaked '{"account" : "bzhnnvqtyzco", "token_code" : "CYBER", "min_own_staked" : 500000000}' -p bzhnnvqtyzco

   cleos push action cyber.stake setkey ‘{“account”:”bzhnnvqtyzco”, “token_code”:”CYBER”, “signing_key”:”GLS7Q3iZkBe4ukfPeeW1iHwaDRu3dh5pzM9KNqDWDHzh5WR9v4wfY”}’ -p bzhnnvqtyzco 

```   
“min_own_staked" : 500000000  - a minimum amount of CYBER tokens required to become a validator.  

Нaving successfully completed the above steps you become a candidate for validators.