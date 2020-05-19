# How To Undelegate Resources

### Goal
Return originally delegated resource for an account.

Beware that only the account which originally delegated resource can undelegate.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is an [account](https://docs.cyberway.io/users/glossary#account);
    * What is a [stake](https://docs.cyberway.io/users/glossary#stake);
    * What is a [bandwidth](https://docs.cyberway.io/users/glossary#bandwidth) in CyberWay;
    * What are [CPU](https://docs.cyberway.io/users/glossary#cpu), [NET](https://docs.cyberway.io/users/glossary#net), [RAM](https://docs.cyberway.io/users/glossary#ram) and [Storage](https://docs.cyberway.io/users/glossary#storage) resources.

### Steps
`alice` account withdraws 10 CYBER from `bob` account which were previously delegated to him:
This operation is performed in two steps.  

*Step_1:* Request for a return of delegated staked tokens.
```sh
$ cleos push action cyber.stake recalluse ‘[alice, bob, “10.0000 CYBER”]' -p alice@active
```

or using specialized cleos system command `undelegatebw`:
```sh
$ cleos system undelegatebw alice bob “10.0000 CYBER”
```

*Step_2:* Crediting withdrawn tokens to a stake.
```sh
$ cleos push action cyber.stake claim ‘[alice, bob, “CYBER”]' -p  alice@active
```

or using specialized cleos system command `claimbw`:
```sh
$ cleos system claimbw alice bob “CYBER”
```

> **Note**  
> The (RAM, NET, CPU, Storage) resources are not directly undelegated. Instead of resources, their total cost is undelegated — number of staked tokens.

### Useful links
  * [The bandwidth in CyberWay](https://docs.cyberway.io/users/bandwidth_implementation#bandwidth-sharing)
  * [The cyberway.stake contract](https://docs.cyberway.io/devportal/system_contracts/cyber.stake_contract)
