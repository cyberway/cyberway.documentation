# 3: Create Development Wallet

This section provides instructions for creating a wallet for use in developing or maintaining software. Before a user can make a change to program code of the product, he/she needs to create a wallet and keys for development.  

**Wallet** — a repository of public-private key pair. Private key is stored in encrypted form and is needed to sign operations performed on the blockchain. Wallet is accessed using `cleos`.  

> **About Wallets**  
> A common misconception in cryptocurrency regarding wallets is that they store tokens. However, in reality, a wallet is used to store private keys in an encrypted file to sign transactions. Wallets do not serve as a storage medium for tokens. Tokens are not stored in the wallets. Wallets store only keys for signing operations.  
> 
> A user builds a transaction object, usually through an interface, sends that object to the wallet to be signed, the wallet then returns that transaction object with a signature which is then broadcast to the network. When/if the network confirms that the transaction is valid, it is included into a block on the blockchain.


## Step 1: Create a Wallet

The first step is to create a wallet. Use `cleos` wallet create to create a new "default" wallet using the option `--to-console` for simplicity. If using `cleos` in production, it is wise to instead use `--to-file` so your wallet password is not in your bash history. For development purposes and because these are development and not production keys `--to-console` poses no security threat.

```sh
$ cleos wallet create --to-console
```

The `cleos` application will return a password to save. Further information appears with a reminder that this password will be needed when unlocking the wallet, and that without a password, it will be impossible to recover the imported keys. 

```sh
Creating wallet: default
Save password to use in the future to unlock this wallet.  
Without password imported keys will not be retrievable.  
"PW5HuQcBBqb...............................kEiE36gweSxy"
```

Running the default command creates a wallet named «default». If you want to create a wallet with a different name (for example, when creating more than one wallet) you can use the `--name` option (or `-n`).
```
$ cleos wallet create --name second-wallet --to-console
```
As a result, a wallet with the `second-wallet` name is created.

## Step 2: Open the Wallet

Wallets are stored in the `keosd` application. Wallets are kept in the closed state by default. To open the wallet, you have to use the `open` operation.
```
$ cleos wallet open
or
$ cleos wallet open --name second-wallet
```
It will return:
```
Opened: default
or
Opened: second-wallet
```

To obtain a list of open wallets use the `list` operation. 
```
$ cleos wallet list
```

It will return:
```
Wallets:
[
  "default",
  "second-wallet"
]
```

In the absence of purses in the open state, the following information will appear:
```
Wallets:
[
]
```

## Step 3: Unlock a Wallet

Despite the fact that the wallet is open, the `keosd` application keeps it in a locked 
state. To use the wallet, it must be unlocked by `unlock` operation.
```sh
$ cleos wallet unlock
or
$ cleos wallet unlock --name second-wallet
```
A password prompt will appear. Enter the password and press `enter`.  

**Note:**  
> You can enter the password directly on the command line by adding the `--password` option. For example,
 
```sh
$ cleos wallet unlock --password PW5...w2
```
You can get a list of open wallets by re-executing: 
```
$ cleos wallet list
```

It should return:  
```sh
Wallets:
[
  "default *",
  "second-wallet *"
]
```
The presence of the (\*) symbol means that the wallet is in the unlocked state.

## Step 4: Import keys into your wallet

After the wallet is created and unlocked, a pair of keys can be loaded into it - private and public. To do this, you can use the `create_key` operation. This operation allows you to generate keys and automatically load them into the wallet. By default, a key with type `K1` — privileged will be generated.
```sh
$ cleos wallet create_key
```
When having more than one wallet, you have specify the name of the wallet in the team, adding the `-name <text>` option.
```sh
$ cleos wallet create_key  --name second-wallet
```

It will return something like the following: 
```sh
Created new private key with a public key of: "GLS8PE...,X6P..."
```
**Please note:**  
> Unlike EOS, in CyberWay the public key code actually starts with the `GLS` characters.

## Step 5: Import the Development Key
Every new CyberWay chain has a default "system" user called "cyber". This account is used to setup the chain by loading system contracts that dictate the governance and consensus of the CyberWay chain. Every new CyberWay chain comes with a development key, and this key is the same. Load this key to sign transactions on behalf of the system user (cyberway)

```sh
$ cleos wallet import
```
You'll be prompted for a private key, enter the cyberway development key provided below:
```sh
5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
```
You now have a default wallet unlocked and loaded with a key, and are ready to proceed

## Step 6: Lock a Wallet (Wallets)
Sometimes it is necessary to have your wallet locked. For instance, when the long-term interruptions in software development are occuring. To lock a single wallet you can use the `lock` operation. 
```sh
$ cleos wallet lock
or
$ cleos wallet lock --name second-wallet
```

A message about locking the user's wallet should appear:
```sh
Locked: 'default'
or
Locked: 'second-wallet'
```

To lock all user wallets, use the `lock_all` operation.
```sh
$ cleos wallet lock_all
```

The following message on locking all user wallets should appear:
```sh
Locked All Wallets
```

## Important

Never use the development key for a production account! Doing so will most certainly result in the loss of access to your account, this private key is publicly known.  


Our congratulations!!! You now have a default wallet unlocked and loaded with a key, and are ready to proceed!!!

****

