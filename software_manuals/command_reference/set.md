# Set

**Description**  
The subcommands can be required to set or update the blockchain state.  

**Subcommands**
 * [Set Code](#set-code) — Create or update the code on an account.
 * [Set Abi](#set-abi) — Create or update the abi on an account.
 * [Set Contract](#set-contract) — Create or update the contract on an account.
 * [Set Account](#set-account) — Set or update blockchain account state.
 * [Set Action](#set-action) — Set or update blockchain action state.

*****
## Set Code

### Description
Create or update the code on an account.

### Positional Parameters
 * `(string) account`— The account to set code for (required).
 * `(string) code-file`— The fullpath containing the contract WASM.


### Options
 * `-c`, `--clear`— Remove code on an account.
 * `--suppress-duplicate-check`— Do not check for duplicate.
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
```
$ cleos set code [OPTIONS] <account> [<code-file>]
```

### Examples
```
$ cleos set code alice ./path/to/wasm
```

## Set Abi

### Description
Create or update the contract on an account.

### Positional Parameters
 * `(string) account`— The account to set the ABI for (required).
 * `(string) abi-file`— The fullpath containing the contract ABI.

### Options
 * `-c`, `--clear`— Remove abi on an account.
 * `--suppress-duplicate-check`— Do not check for duplicate.
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
```
$ cleos set abi [OPTIONS] <account> [<abi-file>]
```

### Examples
```
$ cleos set abi alice ./path/to/abi.abi
```

## Set Contract

### Description
Create or update the contract on an account.

### Positional Parameters
 * `(string) account`— The account to publish a contract for (required).
 * `(string) contract-dir`— The path containing the .wasm and .abi.
 * `(string) wasm-file`— The file containing the contract WASM relative to contract-dir.

### Options
 * `abi-file`, `-a`, `--abi`— The ABI for the contract relative to contract-dir.
 * `-c`, `--clear`— Remove contract on an account.
 * `--suppress-duplicate-check`— Do not check for duplicate.
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
```
$ cleos set contract [OPTIONS] <account> [<contract-dir>] [<wasm-file>]
```

### Examples
Deployind the stake contract.
```
$ cleos set contract currency ../contracts/cyber.stake/stake.wasm ../contracts/cyber.stake/stake.abi
```

## Set Account

### Description
Set parameters dealing with account permissions.

### Positional Parameters
 * `(string) account`— The account to set/delete a permission authority for (required).
 * `(string) permission`— The permission name to set/delete an authority for (required).
 * `(string) authority`— [delete] NULL, [create/update] public key, JSON string or filename defining the authority, [code] contract name.
 * `(string) parent`— [create] The permission name of this parents permission, defaults to 'active'.

### Options
 * `--add-code` — [code] add '${code}' permission to specified permission authority.
 * `--remove-code` — [code] remove '${code}' permission from specified permission authority.
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
To modify the permissions of an account, you must have the authority over the account and the permission of which you are modifying.
```
$ cleos set account permission [OPTIONS] <account> <permission> [<authority>] [<parent>]
```

### Examples
*Example 1*  
Associates a new key to the active permissions of an account.
```
$ cleos set account permission test active '{"threshold" : 1, "keys" : [{"permission":{"key":"GLS7...L6T2s","permission":"active"},"weight":1}], "accounts" : [{"permission":{"account":"bob","permission":"active"},"weight":50}]}' owner
```

*Example 2*  
Modifies the same account permission, but removes the key set in the last example, and grants active authority of the @test account to another account.
```
$ cleos set account permission test active '{"threshold" : 1, "keys" : [], "accounts" : [{"permission":{"account":"sandwich","permission":"active"},"weight":1},{"permission":{"account":"alice","permission":"active"},"weight":50}]}' owner
```

*Example 3*  
Demonstrates how to setup permissions for multisig.
```
cleos set account permission test active '{"threshold" : 100, "keys" : [{"permission":{"key":"GLS7...L6T2s","permission":"active"},"weight":25}], "accounts" : [{"permission":{"account":"@sandwich","permission":"active"},"weight":75}]}' owner
```

## Set Action

### Description
Set parmaters dealing with account permissions.

### Positional Parameters
 * `(string) account`— The account to set/delete a permission authority for (required).
 * `(string) code`— The account that owns the code for the action (required).
 * `(type) type`— The type of the action (required).
 * `(type) requirement`— [delete] NULL, [set/update] The permission name require for executing the given action (required).

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
```
$ cleos set action permission [OPTIONS] <account> <code> <type> <requirement>
```

### Examples
Link a 'voteproducer' action to the 'voting' permissions
```
$ cleos set action permission alice cyber.system voteproducer voting -p alice@voting
```

Now can execute the transaction with the previously set permissions. 
```
$ cleos system voteproducer approve alice someproducer -p alice@voting
```
