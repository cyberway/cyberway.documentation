# How To Link Permission

### Goal
Link a permission to an action of a contract.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Understand the following:
    * What is an [account](https://docs.cyberway.io/users/glossary#account);
    * What is [permission level](https://docs.cyberway.io/users/glossary#permission-level);
    * What is an [action](https://docs.cyberway.io/users/glossary#action).

### Steps
Link a permission level *permlvl* to the action *transfer* of contract *contractname*
```sh
$ cleos set action permission alice contractname transfer permlvl
```