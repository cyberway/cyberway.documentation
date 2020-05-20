# System

**Description**  
The subcommands can be used to send cyber.system contract action to the blockchain.  

**Subcommands**
  * [System Newaccount](#system-newaccount) — Create a new account on the blockchain with initial resources.
  * [System Regproducer](#system-regproducer) — Register a new producer.
  * [System Unregprod](#system-unregprod) — Unregister an existing producer.
  * [System Voteproducer Proxy](#system-voteproducer-proxy) — Vote your stake through a proxy.
  * [System Voteproducer Prods](#system-voteproducer-prods) — Vote for a producer.
  * [System Listproducers](#system-listproducers) — List producers.
  * [System Delegatebw](#system-delegatebw) — Delegate bandwidth.
  * [System Undelegatebw](#system-undelegatebw) — Undelegate bandwidth.
  * [System Claimbw](#system-claimbw) — Claim undelegated bandwidth.
  * [System Listbw](#system-listbw) — List delegated bandwidth.
  * [System Bidname](#system-bidname) — Name bidding.
  * [System Bidnameinfo](#system-bidnameinfo) — Get bidname info.
  * [System Setproxylvl](#system-setproxylvl) — Set an account proxy level.
  * [System Regproxy](#system-regproxy) — Register an account as a proxy (for voting).
  * [System Unregproxy](#system-unregproxy) — Unregister an account as a proxy (for voting).
  * [System Stake](#system-stake) — Stake assets to gain resources.
  * [System Canceldelay](#system-canceldelay) — Cancel a delayed transaction.

*****
## System Newaccount

### Description
Create a new account on the blockchain with initial resources.

### Positional Parameters
 * `(string) creator`— The name of the account creating the new account (required).
 * `(string) name`— The name of the new account (required).
 * `(string) OwnerKey`— The owner public key or permission level for the new account (required).
 * `(string) ActiveKey`— The active public key or permission level for the new account.

### Options
 * `--stake` *TEXT* — The amount of tokens delegated for the account
 * `--transfer`— Transfer voting power and right to unstake tokens to receiver.
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'creator@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system newaccount [OPTIONS] <creator> <name> <OwnerKey> [<ActiveKey>]
```

### Examples
'alice' creates 'bob' and transfers him *50* tokens irrevocably.
```sh
$ cleos system newaccount --stake=50.0000,CYBER --transfer alice bob XXX...XXX
```

## System Regproducer

### Description
Register a new producer.

### Positional Parameters
 * `(string) account` — The account to register as a producer (required).
 * `(string) producer_key` — The producer's public key (required).

### Options
 * `--min-own-stake`— A min value the producer guarantees to stake.
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.


### Command
```sh
$ cleos system regproducer [OPTIONS] <account> <producer_key>
```

## System Unregprod

### Description
Unregister an existing producer.

### Positional Parameters
 * `(string) account` — An account to unregister from producers (required).

### Options
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.


### Command
```sh
$ cleos system unregprod [OPTIONS] <account>
```

## System Voteproducer Proxy

### Description
This subcommand can be used to vote your stake through a proxy.

### Positional Parameters
 * `(string) voter` — The voting account (required).
 * `(string) proxy` — The proxy accoun (required).
 * `(string) quantity` — An asset quantity that the voter delegates to the proxy to vote for the producer (required).

### Options
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'voter@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.


### Command
```sh
$ cleos system voteproducer proxy [OPTIONS] <voter> <proxy> <quantity>
```

## System Voteproducer Prods

### Description
This subcommand can be used to vote for a producer.

### Positional Parameters
 * `(string) voter`— A voting account (required).
 * `(string) producer`— An account to vote for (required).
 * `(string) quantity`— An asset quantity  that the voter votes for the producer (required).

### Options
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'voter@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system voteproducer prods [OPTIONS] <voter> <producer> <quantity>
```

## System Listproducers

### Description
This subcommand can be used to obtain a list of producers.

### Positional Parameters
No parameters required for this subcommand.

### Options
 * `--json`, `-j`— Output in JSON format.
 * `-l`, `--limit` *UINT* — The maximum number of rows to return.
 * `-L`, `--lower` *TEXT* — Lower bound value of key, defaults to first.

### Command
```sh
$ cleos system listproducers [OPTIONS]
```

## System Delegatebw

### Description
This subcommand can be used to delegate bandwidth.

### Positional Parameters
 * `(string) from` — The account to delegate bandwidth from (required).
 * `(string) receiver` — The account to receive the delegated bandwidth (required).
 * `(string) stake_quantity` — The amount of tokens to stake (required).

### Options
 * `--transfer`— Transfer voting power and right to unstake tokens to receiver.
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'from@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.


### Command
```sh
$ cleos system delegatebw [OPTIONS] <from> <receiver> <stake_quantity>
```

### Examples
'alice' delegates *10* tokens to 'bob'
```sh
cleos system delegatebw alice bob "10.0000 CYBER"
```

## System Undelegatebw

### Description
This subcommand can be used to undelegate bandwidth.

### Positional Parameters
 * `(string) from` — The account undelegating bandwidth (required).
 * `(string) receiver` — The account to undelegate bandwidth from (required).
 * `(string) unstake_quantity` — The amount of tokens to undelegate (required).

### Options
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'from@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system undelegatebw [OPTIONS] <from> <receiver> <unstake_quantity>
```

## System Claimbw

### Description
This subcommand can be used to claim undelegated bandwidth.  
This operation can only be performed 30 days after completion of `undelegatebw` (see [Sake Usage Guide](https://docs.cyberway.io/validators/stake_usage_guide#undelegatebw)).

### Positional Parameters
 * `(string) from` — An account claims undelegated stake (required).
 * `(string) receiver` — An account returning stake (required).
 * `(string) token_code` — An asset symbol claimed (required).

### Options
No options required for this subcommand.

### Command
```sh
$ cleos system claimbw <from> <receiver> <token_code>
```
### Examples
'alice' credits the amount of staked tokens returned from 'bob' to a stake.
```sh
$ cleos system claimbw alice bob "CYBER"
```

## System Listbw

### Description
This subcommand can be used to list delegated bandwidth.

### Positional Parameters
 * `(string) account` — The account delegated bandwidth (required).

### Options
 * `--json`, `-j` — Output in JSON format.

### Command
```sh
$ cleos system listbw [OPTIONS] <account>
```

### Examples
'alice' receives a list of users to whom she delegated bandwidth (staked tokens).
```sh
$ cleos system listbw alice
```

## System Bidname

### Description
Name bidding subcommand.

### Positional Parameters
 * `(string) bidder` — The bidding account (required).
 * `(string) newname` — The name which the bid is done for (required).
 * `(string) bid` — The amount of system tokens to bid (it is asset structure) (required).

### Options
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'bidder@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system bidname [OPTIONS] <bidder> <newname> <bid>
```

### Examples
'accountname1' bids *100* tokens to buy 'alice' name at an auction.
```sh
$ cleos system bidname accountname1 alice "100.0000 CYBER"
```

## System Bidnameinfo

### Description
This subcommand can be used to get bidname info.

### Positional Parameters
 * `(string) newname` — The name to lookup to lookup (required).

### Options
 * `--json`, `-j` — Output in JSON format.

### Command
```sh
$ cleos system [OPTIONS] <newname>
```
### Examples
```sh
$ cleos system bidnameinfo alice
```

## System Setproxylvl

### Description
This subcommand can be used to set an account proxy level.

### Positional Parameters
 * `(string) account` — An account whose proxy level will be set (required).
 * `(uint) level` — A proxy level to set (required).

### Options
 * `--symbol` — A symbol of an asset used in the system.
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system setproxylvl [OPTIONS] <account> <level>
```

## System Regproxy

### Description
This subcommand can be used to register an account as a proxy (for voting).

### Positional Parameters
 * `(string) proxy` — A proxy account to register (required).

### Options
 * `--symbol` *TEXT* — A token symbol used by producers.
 * `--level` *UINT* — A proxy level (must be greater than 0, but less than MAX_LEVEL. Default MAX_LEVEL value is *1* ).
 * `--fee` *UINT* — A part of the fee proxies get for a block producing (takes an ineteger value from *0* to *10000* inclusive (*10000* means *100,00* %).
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'proxy@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system regproxy [OPTIONS] <proxy>
```

### Examples
'alice' registers as a proxy having level=1 and fee=50(%).
```sh
$ cleos system regproxy --symbol=4,CYBER --level=1 --fee=5000 alice
```

## System Unregproxy

### Description
Unregister an account as a proxy (for voting).

### Positional Parameters
 * `(string) proxy` — The proxy account to unregister (required).

### Options
 * `--symbol` *TEXT* — A token symbol used by producers.
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'proxy@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system unregproxy [OPTIONS] <proxy>
```

### Examples
'alice' stops registering as a proxy account.
```sh
$ cleos system unregproxy --symbol=4,CYBER alice
```

## System Stake

### Description
This subcommand can be used to stake assets to gain resources.

### Positional Parameters
 * `(string) account` — An account who stakes assets (required).
 * `(string) quantity` — Assets quantity to stake (required).

### Options
 * `--beneficiary` *TEXT* — An account gaining resources.

### Command
```sh
$ cleos system stake [OPTIONS] <account> <quantity>
```

### Examples
```sh
 cleos system stake alice "100.0000 CYBER" --beneficiary=bob
```


## System Canceldelay

### Description
This subcommand can be used to cancel a delayed transaction.

### Positional Parameters
 * `(string) canceling_account` — Account from authorization on the original delayed transaction (required).
 * `(string) canceling_permission` — Permission from authorization on the original delayed transaction (required).
 * `(string) trx_id` — The transaction id of the original delayed transaction (required).

### Options
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'canceling_account@canceling_permission').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos system canceldelay [OPTIONS] <canceling_account> <canceling_permission> <trx_id>
```
