# How To Create An Account

### Goal
Register a name in the system under which transactions can be performed.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * What is an account;
    * What is a public and private key pair.
  * Created an Owner and an Active key pair.
  * Imported a key pair which can authorize on behalf of a creator account.

### Steps
```sh
$ cleos create account creator name OwnerKey [ActiveKey]
```

> **Recommend**  
> ActiveKey is optional but recommended.