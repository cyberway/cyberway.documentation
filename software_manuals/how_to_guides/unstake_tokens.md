# How To Unstake Tokens

### Goal
Withdraw tokens from the stake to active state for your account.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is an account;
    * What is a stake;
    * What is a bandwidth in CyberWay;
    * What are CPU, Network, RAM and Storage resources.

### Steps

Unstake 100 CYBER tokens for `alice` account:
```sh
$  cleos push action cyber.stake withdraw alice “100.0000 CYBER”
```

or using "system withdraw" operation:
```sh
$ cleos system withdraw alice “100.0000 CYBER”
```

Tokens are withdrawn immediately without any delay. After this operation is completed, the `alice` stake will decrease by 100 CYBER tokens and her active tokens will be credited to the `alice` account balance.
