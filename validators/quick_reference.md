
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
 
**Step_1** Create a workspace and execute the commands: 
```
   git clone https://github.com/cyberway/cyberway.launch
   cd cyberway.launch
   sudo ./start_light.sh
```  

Check that  
  * the configuration file has been moved to `/etc/cyberway/config.ini` 
  * the contents of Golos application directory has been moved to `/var/lib/cyberway`.  


**Step_2**  
Specify yours’ account name and both public and private keys in the configuration file `config.ini`. You can specify the keys that you received during registration, or generate new ones by executing
```
cleos wallet create_key
```

Edit variables in the `config.ini` file:
```
signature-provider=<GLS7  … >=KEY:5j****
producer-name=<account name>
```  
 
Run the commands:  
```
   sudo dockerexec -ti nodeosd /bin/bash
   cleos wallet create --to-console
   cleos wallet import --private-key <active-key>

```  

**Step_3**   
The validator candidate steak must be at least 50 000 0000 CYBER tokens. To set the minimum stake, run the command:  
```
    cleos push action cyber.stake setminstaked '{"account" : "<account name>", "token_code" : "CYBER", "min_own_staked" : 50000000}' -p <account name>
```   

The parameter `min_own_staked` is a minimum amount of CYBER tokens required to become a validator.  

**Step_4**  Activate your keys:  

```
   cleos push action cyber.stake setkey ‘{“account”:”<account name>”, “token_code”:”CYBER”, “signing_key”:”<  … >”}’ -p <account name>  
```

**Step_5**  
If the user has not previously been a validator in the blockchain, then he/she needs to set zero proxy level:  
```
    cleos push action cyber.stake setproxylvl '{"account" : "<account name>", "token_code" : "CYBER", "level" : 0}' -p <account name>
```  

```   

Нaving successfully completed the above steps you become a candidate for validators.
