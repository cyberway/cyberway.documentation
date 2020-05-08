# Multisig

**Descriptions**  
Multisig contract commands.  

**Subcommands**
 * [Multisig Propose](#multisig-propose) — Propose action.
 * [Multisig Propose Transaction](#multisig-propose-transaction) — Propose transaction.
 * [Multisig Review](#multisig-review) — Review transaction.
 * [Multisig Approve](#multisig-approve) — Approve proposed transaction.
 * [Multisig Unapprove](#multisig-unapprove) — Unapprove proposed transaction.
 * [Multisig Invalidate](#multisig-invalidate) — Invalidate all multisig approvals of an account.
 * [Multisig Cancel](#multisig-cancel) — Cancel proposed transaction.
 * [Multisig Exec](#multisig-exec) — Execute proposed transaction.
 * [Multisig Schedule](#multisig-schedule) — Schedule delayed proposed transaction.

*****
# Multisig Propose

### Description
Propose action.

### Positional Parameters
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) requested_permissions` — The JSON string or filename defining requested permissions (required).
 * `(string) trx_permissions` — The JSON string or filename defining transaction permissions (required).
 * `(string) contract` — Contract to which deferred transaction should be delivered (required).
 * `(string) action` — Action of deferred transaction (required).
 * `(string) data` —The JSON string or filename defining the action to propose (required). 
 * `(string) proposer` — Account proposing the transaction.
 * `(uint) proposal_expiration` — Proposal expiration interval (in hours), defaults to *24* h.
 * `(string) description` — Optional proposal description.

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig propose <proposal_name> <requested_permissions> <trx_permissions> <contract> <action> <data> [<proposer>] [<proposal_expiration>] [<description>] [OPTIONS]
```

# Multisig Propose Transaction

### Description
Propose transaction.

### Positional Parameters
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) requested_permissions` — The JSON string or filename defining requested permissions (required).
 * `(string) transaction` — "The JSON string or filename defining the transaction to push (required).
 * `(string) proposer` — Account proposing the transaction (author of multisig transaction).
 * `(string) description` — Optional proposal description.

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig propose_trx <proposal_name> <requested_permissions> <transaction> [<proposer>] [<description>] [OPTIONS]
```

# Multisig Review

### Description
Review transaction.

### Positional Parameters
 * `(string) proposer` — Account proposing the transaction (required).
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).

### Options
 * `--show-approvals` — Show the status of the approvals requested within the proposal.

### Command
```
$ cleos multisig review <proposer> <proposal_name> [OPTIONS]
```


# Multisig Approve

### Description
Approve proposed transaction.

### Positional Parameters
 * `(string) proposer` — Account proposing the transaction (required).
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) permissions` — The JSON string of filename defining approving permissions (required).
 * `(string) proposal_hash` — Hash of proposed transaction (i.e. transaction ID) to optionally enforce as a condition of the approval.

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig approve <proposer> <proposal_name> <permissions> [<proposal_hash>] [OPTIONS]
```


# Multisig Unapprove

### Description
Unapprove proposed transaction.

### Positional Parameters
 * `(string) proposer` — Account proposing the transaction (required).
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) permissions` — The JSON string of filename defining approving permissions (required).

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig unapprove <proposer> <proposal_name> <permissions> [OPTIONS]
```


# Multisig Invalidate

### Description
Invalidate all multisig approvals of an account. Revoke all permissions previously issued by the account for performing multisig transactions. The action applies to all proposed transactions that are at the voting stage.

### Positional Parameters
 * `(string) invalidator` — Invalidator name whose previously issued permissions to perform multisig transactions must be invalidated (required).

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig invalidate <invalidator> [OPTIONS]
```

# Multisig Cancel

### Description
Cancel proposed transaction.

### Positional Parameters
 * `(string) proposer` — Account proposing the transaction (required).
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) canceler` — Canceler name that cancels the execution.

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig cancel <proposer> <proposal_name> [<canceler>] [OPTIONS]
```

# Multisig Exec

### Description
Execute proposed transaction.

### Positional Parameters
 * `(string) proposer` — Account proposing the transaction (required).
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) executer` — account paying for execution.

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout`).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.
 
### Command
```
$ cleos multisig exec <proposer> <proposal_name> [<executer>] [OPTIONS]
```


# Multisig Schedule

### Description
Schedule delayed proposed transaction.

### Positional Parameters
 * `(string) proposer` — Account proposing the transaction (required).
 * `(string) proposal_name` — The unique name assigned to the multisig transaction when it is created (required).
 * `(string) actor` — account paying for scheduling.

### Options
 * `-x,--expiration` *TEXT* — Set the time (in seconds) before a transaction expires, defaults to *30* s.
 * `-f,--force-unique` — Force the transaction to be unique. This will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times.
 * `-s,--skip-sign` — Specify if unlocked wallet keys should be used to sign transaction.
 * `-j,--json` — Print result as JSON.
 * `-d,--dont-broadcast` — Do not broadcast transaction to the network (just print to `stdout` ).
 * `--return-packed` — Used in conjunction with `--dont-broadcast` to get the packed transaction.
 * `-r,--ref-block` *TEXT* — Set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake).
 * `-p,--permission` *TEXT* — An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active').
 * `--max-cpu-usage-ms` *UINT* — Set an upper limit on the milliseconds of CPU usage budget, for the execution of the transaction (defaults to *0* which means no limit).
 * `--max-net-usage` *UINT* — Set an upper limit on the net usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-ram-usage` *UINT* — Set an upper limit on the ram usage budget (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--max-storage-usage` — Set an upper limit on the storage usage budget, (in bytes) for the transaction (defaults to *0* which means no limit).
 * `--delay-sec` *UINT* — Set the `delay_sec` seconds, defaults to *0* s.
 * `--bandwidth-provider` *TEXT* — Set an account which provide own bandwidth for transaction.
 * `--dont-declare-names` — Do not add `declarenames` action for resolved account names.

### Command
```
$ cleos multisig schedule <proposer> <proposal_name> [<actor>] [OPTIONS]
```
