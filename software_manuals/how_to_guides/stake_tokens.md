# How To Stake Tokens

### Goal
Stake tokens for your account.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is an account;
    * What is a bandwidth in CyberWay;
    * What are CPU, Network, RAM and Storage resources.

### Steps

Stake 100 CYBER tokens for `alice` account:
```sh
$ cleos push action cyber.token transfer ‘[alice, cyber.stake, “100.0000 CYBER”] -p alice@active
```

or using "system stake" operation:
```sh
$ cleos system stake alice “100.0000 CYBER”
```

> **Note**  
> When you perform a transaction, you do not need to worry about which specific resource (RAM, NET, CPU or Storage) will be consumed more (or less) and how the staked tokens should be spent. The system dynamically and optimally distributes your staked tokens.
