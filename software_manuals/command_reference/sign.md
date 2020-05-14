# Sign

### Description
The subcommand can be used to sign a transaction.

### Positional Parameters
 * `(string) transaction`— The JSON string or filename defining the transaction to sign (required).

### Options
 * `-k`, `--private-key` *TEXT* — The private key that will be used to sign the transaction.
 * `-c`, `--chain-id` *TEXT* — The chain id that will be used to sign the transaction.
 * `-p`, `--push-transaction` — Push transaction after signing.

### Command
```
$ cleos sign [OPTIONS] <transaction>
```
