

*Dear users of the CyberWay platform! The @cyberwaydev team is pleased to provide you with a document explaining the main points regarding validators as a special category of users on our blockchain. The document contains the policies regarding the CyberWay validators, and provides information on their functions, as well as factors affecting the profitability of validator’s actions.*

## Terminology used
**Votes cast for a validator** — votes represent a certain number of tokens staked for a validator during the voting process (one token equals to one vote).  

**Stake** — a share of bandwidth resources (RAM, NET, CPU and Storage) allocated to a user. The user can manage the share of resources allocated to him both independently and entrust its use to another user (delegate the share of resources).  

**Staked tokens** — the tokens allocated for a stake acquisition that can’t be used for anything else in this state. The user can stake active tokens listed on his/her balance or deposit them. Also, the user can perform the reverse operation — withdraw tokens from the staked state to active.  

**Proxy account** — a user empowered with a specific voting authorization during the voting process. A proxy account can be declared by any user who is ready to accept votes from other users and vote for the validators on their behalf.  

**Proxy account level** — a conditional division of users into categories. The highest level of proxy account is zero, which is assigned only to validators. The first and further levels in ascending order are assigned to users who have declared themselves proxy accounts. The number of levels (categories) of proxy accounts is not limited. The number of proxy accounts of the same level is also not limited. The last level of proxy account is assigned to the ordinary user. In the current release, the number of proxy account levels is limited to three. A proxy account can only be configured at one of the following levels:
  * A validator is considered as a zero-level proxy account.
  * First level proxy account - a user who has declared himself a proxy account, empowered with the votes of ordinary users at his disposal during the voting process.
  * The second level proxy account is an ordinary user. The ordinary user does not have to worry about what level of proxy he/she needs to set. The user is automatically assigned to the lowest-rated proxy level.  

The number of parts into which tokens can be divided and, accordingly, the number of validators for which you can simultaneously vote entirely depends on the proxy level of your account. In the current implementation, the users of the first proxy level can simultaneously vote for one or more validators (the maximum number is 30). The users of the second proxy level can vote for one validator or one proxy account only. A user sets stakes tokens for each of the validators he/she votes for. The user of the zero proxy level (validator) does not participate in the voting.


## The role and objectives of validators in the system

***Validators are users who provide reliable and safe functioning of network nodes, production of chain blocks and govern the blockchain and manage the development of the blockchain.
Important! Validators provide their equipment for use to the network.***


### Validator duties
Validators have to perform the following functions:
  * to deploy and configure his/her network node;
  * to configure the network settings based on validator voting, network resource allocation (CPU, NET, RAM, Storage);
  * to produce and sign chain blocks;
  * to update software and system contracts;
  * to make decisions of tokens issueв or burned;
  * to vote on decisions related to the further development of the blockchain.



### Main regulations

**Validator status**
The status of a validator candidate can be obtained by any user who has signed up, switched to a zero proxy level and has his own stake in the amount of at least 50,000 CYBER as well as a server (node) that meets the requirements specified in [manual](https://cyberway.gitbook.io/en/validators/quick_reference).
The validators are selected from the candidates by users via voting (read more about the voting process [below](#the-voting-process-conducted-by-the-user))
According to the voting results, the validators are divided into two groups - the key validators those who receives the most votes and reserve candidates.

**Active validators**
  * Block production-wise, a schedule of active validators is automatically compiled, which includes all the main validators and one reserve candidate.
  * The selection of a reserve candidate for the schedule of active validators takes place according to the following rules. The priority of the candidate is calculated in accordance with the established formula, taking into account the number of votes cast for him and the time interval since his last appearance in the schedule of active validators.
  * Active validators produce blocks alternately according to the schedule.
  * The schedule of active validators is updated periodically every 4×A blocks, where A is the current number of active validators.
The number of active validators changes upwards from 21 to 101 inclusive. The number of active validators increases by one if two conditions are met:
    * at least 14 days have passed since the last increase in the number of active validators;
    * the number of votes cast for the main validators was less than 90 % of the total number of votes cast for all candidates.

**Validator rewards**
  * The validator receives a reward for each block he/she signs.
  * A compensation is accrued on every 3499th block produced.
  * A reward in the form of CYBER staked tokens is automatically calculated and remains on the balance of the validator. The transfer of these tokens to the liquid state is performed manually (see [below](#converting-the-tokens-into-the-staked-ones)).
  * The reward amount depends on the following parameters:
    * a share of reward to the validators from the annual issuance of Cyber tokens;
    * a share of reward that is distributed between validators and users who voted for them;
    * a number of key validators at the moment;
    * a number of skipped blocks.

  * The funds from the annual issuance of tokens are distributed in the following proportions:
    * 10 % goes to reward pool for validators for block production;
    * 20 % goes to reward pool for workers;
    * 70 % goes to the stake pool that is distributed between validators and users that voted for them.

  * Rewards to validators  are distributed in the following proportions: 
    * 10 % from the validators pool goes to a validator, as well as commissions that he/she sets, which are taken from the stake pool;
    * the rest of the stake pool is distributed between the proxy and generic accounts that voted for this specific validator.

  * The CyberWay annual inflation depends on the number of staked tokens used in a voting for validators. The following token emission values are set in the current implementation:
    * the inflation is 20 % if the number of tokens in the vote does not exceed 25 %;
    * the  inflation is 10 % if the number of tokens in the vote exceeds 75 %;
    * the inflation decreases linearly with an increase in the number of tokens used in voting from 25 to 75 %.



## How to become a candidate for a validator

To become a candidate check our [brief guide](https://cyberway.gitbook.io/en/validators/quick_reference) that outlines step-by-step actions a user needs to follow during registration and presents a list of basic node characteristics.

### Setting a Fee  
A validator, as well as a candidate for validators, is required to set a `fee`, which will be allocated to him/her from the stake pool reward in the future.  An unreasonably high `fee` may reduce the number of users voting for the validator. On the contrary, an unreasonably low `fee` may not compensate for the validator's costs for node maintenance. Therefore, the validator should be most responsible for setting the commission.  

To set `fee`, the validator can use the following `cleos` command:
```sh
    cleos push action cyber.stake setproxyfee '{"account" : <account>, "token_code" :<token_code>, "fee" : <fee>}' -p <account>
```

Command line arguments:
  * `account` — validator account.
  * `token_code` — token symbol that make up the reward.
  * `fee` — the amount of the commission indicated with an accuracy of hundredths of a percent. This parameter takes values "0-10000". The "0" value corresponds to "0 %", and "10000" corresponds to "100 %".  

The example with setting the "7 %" `fee`  by `alice` validator:
```sh
$ cleos push action cyber.stake setproxyfee '{"account":"alice", "token_code":"CYBER", "fee":700}' -p alice
```

### Setting proxy level  

The validator as well as the candidate for validators must always remain at zero proxy level. You may use the following `cleos` command to configure the proxy level to zero:
```sh
$ cleos system setproxylvl <account> <level>
```
Command line arguments:
  * `account` — validator account.
  * `level` — the proxy level to be set. This parameters must always remain as "0".  

The following example demonstrates the way the zero proxy level is configured for the user called `alice`:
```sh
$ cleos system setproxylvl alice 0
```

### Setting public key  
The validator must hold a public key (his own digital signature) for block signing. You may use the following `cleos` command to activate the validator key:
```sh
$ cleos push action cyber.stake setkey '{"account":"<account>", "token_code":"CYBER", "signing_key":"<... digital signature...>"}' -p <account>
```
Command line arguments:
  * `account` — validator to which the public key is set.
  * `token_code` — staked token code.
  * `signing_key` — public key (validator’s signature) to be set.  

The following example demonstrates setting the public key "PW5Kewn9L...t42S9XCw2" by `alice` validator, which she will use when signing blocks:
```sh
$ cleos push action cyber.stake setkey '{"account":"alice", "token_code":"CYBER", "signing_key":"GLS7M53j...6FAx8Y881"}' -p alice
```  

### Setting both public key and proxy level via one command
The user also has the ability to set both the proxy level to zero and the public key simultaneously by using one command:
```sh
$ cleos system regproducer <account> <signing_key>  --min-own-stake <value>
```

Unlike the previous command, the optional argument "--min-own-stake <value>" can be set here, which specify the minimum number of staked tokens that a user must have in order to be a candidate for validators.  

Example of declaring user `alice` as a validator candidate:
```sh
$ cleos system regproducer alice GLS7M53j...6FAx8Y881 --min-own-stake 500000000
```



## Voting for validators
A user can vote for validators both independently either via proxy account, entrusting the latter with his voting authorization.

### The voting process conducted by the user

First, register on golos.io and go to the ["Validators"](https://golos.io/validators) page. Then:
  * select the up arrow icon in front of the validator or candidate;
  * indicate the number of CYBER tokens to be staked;
  * confirm the transaction with a password or an active key.  

If the decision needs to be changed, or the number of staked tokens have to be decreased do the following:
  * select the down arrow icon in front of the validator of interest;
  * set the required percentage of tokens to be decreased (or increased) using the slider;
  * click the "divide" button;
  * confirm the transaction with a password or an active key.


### Voting via proxy account

Regardless of reasons implied, a user not willing to participate in the voting for validators can use the services of a proxy account and entrust it with a right to vote. To do this, the user needs to delegate the staked Cyber tokens to a proxy account. 

To delegate staked Cyber tokens to a proxy account, you can use the following `cleos` command:
```sh
$ cleos system voteproducer prods <user account> <proxy account> "quantity CYBER"
```
Command line arguments:
  * `user account` — the name of the account that delegates the steak (or part of the stake) to the proxy account.
  * `proxy account` — proxy account to which tokens are delegated.
  * `quantity CYBER` — the number of staked tokens that the proxy account can use when voting.  
  
The example with the delegation of 100 CYBER Stake tokens to the `bob` proxy account by the `alice` user:

```sh
$ cleos system voteproducer prods alice bob "100.0000 CYBER" 
```

 
### Declaring yourself as a proxy account
A user can declare himself a proxy account and vote for validators with the staked Cyber tokens. To become a proxy account, the user has to set a proxy level to "one" (note: only "one" can be set in the current implementation. in further implementations, the proxy account level can be 1, 2 or more).  

To set the proxy account level, you can use the following `cleos` command:
```sh
$ cleos system setproxylvl <account> <level>
```
Command line arguments:
  * `user account` — the name of the account for which the proxy account level is set
  * `level` —  proxy level to be set
  * `active key` — the active key of the user

The example with setting the proxy level "1" to a user with the `alice` name:
```sh
$ cleos system setproxylvl alice 1
```


### Converting the tokens into the staked ones
To increase the number of staked tokens, it is necessary to convert CYBER tokens to the staked CYBER tokens.  

For that you can use the following cleos command:
```sh
$ cleos system stake <user account> <quantity CYBER>
```
Command line arguments:
  * `user account` — the name of the account that transfers tokens to the steak.
  * `quantity CYBER` — the number of tokens being transferred.  

The example with the transfer of 100 CYBER tokens to CYBER Stake tokens by user `alice`:

```sh
$ cleos system stake alice "100.0000 CYBER"
```

### The unvoting procedure (unstaking, or withdrawal of staked tokens)

A user can perform the reverse operation by converting staked CYBER tokens to CYBER ones. First the votes from the validators must be recalled:

  * enter the explorer;
  * add the account name in the url;
  * when on the account page, choose the validator you want to unvote and click the "Recall" button.  

Next, convert the staked CYBER tokens to liquid ones. To do this, use the following command:
```sh
$ cleos push action cyber.stake withdraw '[<account>, <unstake quantity>]' -p <account>
```
Command line arguments:
  * `account` — account name that converts CYBER Stake tokens to CYBER.
  * `unstake quantity` — number of tokens being converted.  

Some staked CYBER tokens can be blocked for resources used that will be restored only within a certain time (for example, a month is spent on restoring Storage resources).  

Example. The user `alice` converts 100 CYBER Stake tokens into CYBER tokens.
```sh
$ cleos push action cyber.stake withdraw '[alice, "100.0000 CYBER"]' -p alice
```

## Conclusion

Currently the users can perform all actions regarding validators, namely could become validators, could vote for the validators, or unvote them via CyberWay explorer.

