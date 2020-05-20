# Transfer

### Description
The subcommand can be used to transfer tokens from account to account.

### Positional Parameters
 * `(string) sender` — The account sending tokens (required).
 * `(string) recipient` — The account receiving tokens (required).
 * `(string) amount` — The amount of tokens to send (required).
 * `(string) memo` — The memo for the transfer.

### Options
 * `--contract`, `-c` *TEXT* — The contract which controls the token.
 * `--pay-ram-to-open` — Pay ram to open recipient's token balance row.
 * `-x`, `--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f`, `--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s`, `--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j`, `--json` — Print result as JSON.
 * `-d`, `--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r`, `--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p`, `--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'sender@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```sh
$ cleos transfer [OPTIONS] <sender> <recipient> <amount> [<memo>]
```

### Examples
'alice' transfers *100* CYBER to 'bob'.
```sh
$ cleos transfer alice bob "100.0000 CYBER" "use for voting only"
```
