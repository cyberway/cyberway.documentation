# How To List All Key Pair

### Goal
List all key pairs.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * What is a public and private key pair.

### Steps
Unlock your wallet
```sh
$ cleos wallet unlock
```

List all public keys:
```sh
$ cleos wallet keys
```

List all private keys:
```sh
$ cleos wallet private_keys
```

You can enter the password directly on the command line by adding the `--password` option:
```sh
$ cleos wallet unlock --password PW5...3RwP2
```

> Be careful, never use your private keys in a production enviroment.