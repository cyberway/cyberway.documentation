# How To Stake Tokens

### Goal
Stake tokens for your account.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is an [account](https://docs.cyberway.io/users/glossary#account);
    * What is a  [stake](https://docs.cyberway.io/users/glossary#stake);
    * What is a [bandwidth](https://docs.cyberway.io/users/glossary#bandwidth) in CyberWay;
    * What are [CPU](https://docs.cyberway.io/users/glossary#cpu), [NET](https://docs.cyberway.io/users/glossary#net), [RAM](https://docs.cyberway.io/users/glossary#ram) and [Storage](https://docs.cyberway.io/users/glossary#storage) resources.

### Steps

Stake 100 CYBER tokens for `alice` account:
```sh
$ cleos push action cyber.token transfer ‘[alice, cyber.stake, “100.0000 CYBER”]' -p alice@active
```

or using "system stake" operation:
```sh
$ cleos system stake alice “100.0000 CYBER”
```

> **Note**  
> When you perform a transaction, you do not need to worry about which specific resource (RAM, NET, CPU or Storage) will be consumed more (or less) and how the staked tokens should be spent. The system dynamically and optimally distributes your staked tokens.

### Useful links
  * [The bandwidth in CyberWay](https://docs.cyberway.io/users/bandwidth_implementation#bandwidth-sharing)
  * [The cyberway.stake contract](https://docs.cyberway.io/devportal/system_contracts/cyber.stake_contract)
