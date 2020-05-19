# How To Unlink Permission

### Goal
Unlink a linked permission level.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * What is an [account](https://docs.cyberway.io/users/glossary#account);
    * What is [permission level](https://docs.cyberway.io/users/glossary#permission-level);
    * What is an [action](https://docs.cyberway.io/users/glossary#action).

### Steps
Remove a linked permission level from an action *transfer* of contract *contractname* for `alice` account:

```sh
$ cleos set action permission alice contractname transfer NULL
```