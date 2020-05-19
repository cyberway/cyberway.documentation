# How To Delegate Resources

### Goal
Delegate resource for an account.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is an [account](https://docs.cyberway.io/users/glossary#account);
    * What is a [stake](https://docs.cyberway.io/users/glossary#stake);
    * What is a [bandwidth](https://docs.cyberway.io/users/glossary#bandwidth) in CyberWay;
    * What are [CPU](https://docs.cyberway.io/users/glossary#cpu), [NET](https://docs.cyberway.io/users/glossary#net), [RAM](https://docs.cyberway.io/users/glossary#ram) and [Storage](https://docs.cyberway.io/users/glossary#storage) resources.

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

### Useful links
  * [The bandwidth in CyberWay](https://docs.cyberway.io/users/bandwidth_implementation#bandwidth-sharing)
  * [The cyberway.stake contract](https://docs.cyberway.io/devportal/system_contracts/cyber.stake_contract)
