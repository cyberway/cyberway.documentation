# Push

**Description**  
The subcommands can be used to push arbitrary transactions to the blockchain.  

**Subcommands**
 * [Push Action](#push-action) — Retrieve an account from the blockchain.
 * [Push Transaction](#push-transaction) — Retrieve accounts associated with a public key.
 * [Push Transactions](#push-transactions) — Retrieve accounts associated with a public key.

*****
## Push Action

### Description
Push a transaction with a single action.

### Positional Parameters
 * `(string) account`— The account providing the contract to execute (required).
 * `(string) action`— A JSON string or filename defining the action to execute on the contract (required).
 * `(string) data`— The arguments to the contract (required).

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission'.
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos push [OPTIONS] <account> <action> <data>
```


## Push Transaction

### Description
Push an arbitrary JSON transaction.

### Positional Parameters
 * `(string) transaction`— The JSON string or filename defining the transaction to push (required).

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission'.
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos push transaction [OPTIONS] <transaction>
```


## Push Transactions

### Description
Push an array of arbitrary JSON transactions.

### Positional Parameters
 * `(string) transaction`— The JSON string or filename defining the array of the transactions to push (required).

### Options
No options required fot this subcommand.

### Command
Pushes an array of arbitrary JSON transactions.
```sh
$ cleos push transactions {<transaction1>...<transactionN>}
```
