# Wallet

**Descriptions**  
The subcommands can be used to interact with local wallet.  

**Subcommands**
  * [Wallet Create](#wallet-create) — Create a new wallet locally.
  * [Wallet Open](#wallet-open) — Open an existing wallet.
  * [Wallet Lock](#wallet-lock) — Lock wallet.
  * [Wallet Lock All](#wallet-lock-all) — Lock all unlocked wallets.
  * [Wallet Unlock](#wallet-unlock) — Unlock wallet.
  * [Wallet Import](#wallet-import) — Import private key into wallet.
  * [Wallet Remove Key](#wallet-remove-key) — Remove key from wallet.
  * [Wallet Create Key](#wallet-create-key) — Create private key within wallet.
  * [Wallet List](#wallet-list) — List opened wallets.
  * [Wallet Keys](#wallet-keys) — List of public keys from all unlocked wallets.
  * [Wallet Private Keys](#wallet-private-keys) — List of private keys from an unlocked wallet in WIF or PVT_R1 format.

# Wallet Create

### Description
This subcommand creates a wallet with the specified name. If no name is given, the wallet will be created with the name 'default'.

### Positional Parameters
None.

### Options
 * `-n`, `--name` *TEXT* — The name of the new wallet.
 * `-f`, `--file` *TEXT* — Name of file to write wallet password output to (must be set, unless `--to-console` is passed).
 * `--to-console` — Print password to console.

### Command
```
$ cleos wallet create [OPTIONS]
```

### Examples
```
$ cleos wallet create --name=walletname1 --to-console
```

# Wallet Open

### Description
This subcommand can be used to open an existing wallet.

### Positional Parameters
None.

### Options
 * `-n`, `--name` *TEXT* — The name of the wallet to open.

### Command
```
$ cleos wallet open [OPTIONS]
```

### Examples
*Example 1*
```
$ cleos wallet open
```

*Example 2*
```
$ cleos wallet open --name=walletname1
```

*Output*
```
Opened: walletname1
```

# Wallet Lock

### Description
This subcommand can be used to lock wallet.

### Positional Parameters
None.

### Options
 * `-n`, `--name` *TEXT* — The name of the wallet to lock.

### Command
```
$ cleos wallet lock [OPTIONS]
```

### Examples
*Example 1*
```
$ cleos wallet lock
```

*Example 2*
```
$ cleos wallet lock --name=walletname1
```

*Output*
```
Locked: walletname1
```

# Wallet Lock All

### Description
This subcommand can be used to lock all unlocked wallets.

### Positional Parameters
None.

### Options
None.

### Command
```
$ cleos wallet lock_all
```

*Output*
```
Locked All Wallets
```

# Wallet Unlock

### Description
This subcommand can be used to unlock wallet.

### Positional Parameters
None.

### Options
 * `-n`, `--name` *TEXT* — The name of the wallet to unlock.
 * `--password` *TEXT* — The password returned by [wallet create](#wallet-create).

### Command
```
$ cleos wallet unlock [OPTIONS]
```

### Examples
To unlock a wallet, specify the password provided when it was created.
```
$ cleos wallet unlock --name=walletname1 --password=XXXXXXXX
```

*Output*
```
Unlocked: walletname1
```

# Wallet Import

### Description
This subcommand can be used to import private key into wallet.

### Positional Parameters
None.

### Options
 * `-n,--name` *TEXT* — The name of the wallet to import key into.
 * `--private-key` *TEXT* — Private key in WIF format to import.

### Command
```
$ cleos wallet import [OPTIONS]
```

### Examples
```
$ cleos wallet import --name=walletname1 --private-key=3Rft...Uh6
```

# Wallet Remove Key

### Description
This subcommand can be used to remove key from wallet.

### Positional Parameters
 * `(string) key` — Public key in WIF format to remove (required).

### Options
 * `-n`, `--name` *TEXT* — The name of the wallet to remove key from.
 * `--password` *TEXT* — The password returned by [wallet create](#wallet-create).

### Command
```
$ cleos wallet remove_key <key> [OPTIONS]
```

### Examples
```
$ cleos wallet remove_key GLS8PE...rS3T --name=walletname1 --password=XXXXXXXX
```

# Wallet Create Key

### Description
This subcommand can be used to creates a key pair within the wallet so that you don't need to manually import it like you would with cleos create key. By default, this will create a key with the type "favored" by the wallet, which is a "K1" key. This subcommand also lets you create a key in "R1" format.

### Positional Parameters
 * `(string) key_type` — "K1" or "R1" key type to create. "K1" generates privileged key. Defaults to "K1".

### Options
 * `-n`, `--name` *TEXT* — The name of the wallet to create key into.

### Command
```
$ cleos wallet create_key [OPTIONS]
```

### Examples
```
$ cleos wallet create_key K1 --name-walletname1
```
```
Created new private key with a public key of: "GLS8PE...6tR, X6P...5EfG"
```

# Wallet List

### Description
The subcommand can be used to list opened wallets ("*" means "unlocked").

### Positional Parameters
None.

### Options
None.

### Command
```
$ cleos wallet list
```

*Output*
```
Wallets:
[
  "default *",
  "walletname1 *"
]
```

or when there are no wallets:
```
Wallets:
[
]
```

# Wallet Keys

### Description
The subcommand can be used to list of public keys from all unlocked wallets. These are the keys that could be used to sign transactions.

### Positional Parameters
None.

### Options
None.

### Command
```
$ cleos wallet keys
```
*Output*
```
[[
    "GLS6MR...qcVpscN",
    "PbwdL6...FSSwsST3"
  ]
]
```

### Examples
```
$ cleos wallet 
```

# Wallet Private Keys

### Description
The subcommand can be used to list of private keys from an unlocked wallet in WIF or PVT_R1 format.  
It is possible to query for the public and private key pairs of an individual wallet. The wallet must already be unlocked and you must give the password again.

### Positional Parameters
None.


### Options
 * `-n`, `--name` *TEXT* — The name of the wallet to list keys from.
 * `--password` *TEXT* — The password returned by wallet create.

### Command
```
$ cleos wallet private_keys [OPTIONS]
```

### Examples
```
$ cleos wallet private_keys --name-walletname1 --password=XXXXXXXX
```