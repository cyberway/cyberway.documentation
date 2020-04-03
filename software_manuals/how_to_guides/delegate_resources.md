# How To Delegate Resources

### Goal
Delegate resource for an account.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is an account;
    * What is a stake;
    * What is a bandwidth in CyberWay;
    * What are CPU, Network, RAM and Storage resources.

### Steps

`alice`  delegates 100 tokens to `bob`. This operation is signed by the alice's active key:
```sh
$ cleos push action cyber.stake delegateuse ‘[alice, bob, “10.0000 CYBER”]` -p alice@active
```
or using specialized cleos system command `delegatebw`:
```sh
$ cleos system delegatebw alice bob “100.0000 CYBER”
```

> **Note**  
> The (RAM, NET, CPU, Storage) resources are not directly delegated. Instead of resources, their total cost is delegated — number of staked tokens.

