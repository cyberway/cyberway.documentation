# How To Unlink Permission

### Goal
Unlink a linked permission level.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * What is an account;
    * What is permission level;
    * What is an action.

### Steps
Remove a linked permission level from an action *transfer* of contract *contractname* for `alice` account:

```sh
$ cleos set action permission alice contractname transfer NULL
```