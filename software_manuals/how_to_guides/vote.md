# How To Vote

### Goal
Vote for a validator.

### Before you begin
  * Install the currently supported version of `cleos`.
  * Ensure the reference system contracts from `cyberway.contracts` repository is deployed and used to manage system resources.
  * Understand the following:
    * What is a [validator](https://docs.cyberway.io/users/glossary#validator);
    * What is a [proxy account](https://docs.cyberway.io/users/glossary#proxy-account);
    * How does voting works.
  * Unlock your wallet.

### Steps
*Option_1:* Vote yourself.
`alice` votes for the`bob` validator and allocates 50 CYBER tokens for it:
```sh
$ cleos system voteproducer prods alice bob “50.0000 CYBER”
```

*Option_2:* Vote via a proxy account.
If you are unable to vote, you can entrust your vote to a proxy account that will vote for you. To do this, you need to delegate the staked tokens to a proxy account.  

`alice` delegates  50 CYBER tokens to `bob` that is a proxy account:
```sh
$ cleos system voteproducer prods alice bob “50.0000 CYBER”
```

### Useful links
  * [Validators in CyberWay](https://docs.cyberway.io/validators/voting_for_validators#the-role-and-objectives-of-validators-in-the-system)
  * [Voting for validators](https://docs.cyberway.io/validators/voting_for_validators#voting-for-validators)
