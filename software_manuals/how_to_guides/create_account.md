# How To Create An Account

### Goal
Register an account in the system and delegate staked tokens to it so that this account can perform transactions.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * What is an [account](https://docs.cyberway.io/users/glossary#account);
    * What is a [public](https://docs.cyberway.io/users/glossary#public-key) and [private](https://docs.cyberway.io/users/glossary#private-key) key pair.
  * Created an Owner and an Active key pair.
  * Imported a key pair which can authorize on behalf of a creator account.

### Steps
User `alice` creates the `bob` account name and transfers to him 100 CYBER tokens:
```sh
$ cleos system newaccount alice bob "100.0000 CYBER"
```

If the `--transfer` flag is added to command line then staked tokens will irrevocably be transferred to created account:
```sh
$ cleos system newaccount alice bob "100.0000 CYBER" --transfer
```
