# Govern Smart Contract

## Purpose of the cyber.govern smart contract development
 
The `cyber.govern` system smart contract contains the logic for selecting active validators and distributing remuneration for block production in accordance with the DPoS provisions.  

The actions supported:
 * [onblock](#onblock)
 * [setshift](#setshift)

The events supported:
 * [burnreward](#burnreward)

## Terminology used

**Cycle** - the time spent on one queue in a schedule (if the schedule contains N vaditors, then N blocks per 1 cycle must be produced).  

**Round** - the time it takes to complete four cycles (the time for which *4×N* blocks must be produced, where *N* is number of schedule validators).

## onblock

The `onblock` action is a system operation and is not accessible to an external user. It is called by the `cyber.bios` smart contract. The operation contains the logic of block production, including the logic of token issuance and the appointment of validators. This action has the following form:
```
[[eosio::action]] void onblock(name producer);
```
`producer` parameter is a name of validator that succeed producing the next block.  

The `cyber.bios` smart contract activates the `onblock` action after each block produced by validators. Calling `onblock` requires  authorization of the `cyber.govern` smart contract account.  

## setshift
This action allows validators to set a rate of change in their number. When assigning a new list of validators, their number will be changed to value specified in shift if the last change (or `setshift` call) was later than schedule_resize_min_delay (14 days).  

```cpp
[[eosio::action]] void setshift(int8_t shift)
```
`shift` parameter sets the rate of change in a number of validators. Valid values ​​limited by contract are (-1, 0, +1).

To perform the action, authorization of a `cyber.prods` account is required.


## burnreward
The `burnreward` event reports on burning an award allocated to a validator. A validator is not rewarded if no one block was produced by him during one round. The highlighted reward to validators is marked on their special account at the beginning of the round. When the validator produces a block, he marks these funds as requested and at the beginning of the next round they are transferred to a validator account. If the validator skips block production, then this award is burned.

```cpp
[[eosio::event]] void burnrevard(
    name      account,
    int64_t   amount
)
```

**Parameters:**  
  * `account` — validator account whose funds are burned.
  * `amount` — amount of funds burned.


## Main regulations concerning the validators

### Validator status

  * The status of a validator candidate can be obtained by any user who has signed up, switched to a zero proxy level and has his own stake in the amount of at least 50,000 CYBER as well as a server (node) that meets the requirements specified in [manual](https://cyberway.gitbook.io/en/validators/quick_reference).
  * The validators are selected from the candidates by users via voting (read more about [voting process](https://cyberway.gitbook.io/en/validators/voting_for_validators)).
  * The validators are divided into two groups according to the voting results — the main validators who received the biggest number of votes and reserved candidates.
  
### Active validators

  * Block production-wise, a schedule of active validators is automatically compiled, which includes all the main validators and one reserve candidate.
  * The selection of a reserve candidate for the schedule of active validators takes place according to the following rules. The priority of the candidate is calculated in accordance with the established formula, taking into account the number of votes cast for him and the time interval since his last appearance in the schedule of active validators.
  * Active validators produce blocks alternately according to the schedule.
  * The schedule of active validators is updated periodically every 4×A blocks, where A is the current number of active validators.
  * The number of active validators changes upwards from 21 to 101 inclusive. 
  * The number of active validators increases by one if two conditions are met:
    * at least 14 days have passed since the last increase in the number of active validators;
    * the number of votes cast for the main validators was less than 90 % of the total number of votes cast for all candidates.  


### Validator rewards

  * The validator is rewarded for blocks produced by him in accordance with the provisions of DPoS and taking into account the annual issue of tokens. Funds generated from the annual issue of tokens are distributed in the following fractions:
    * 10 % goes to reward pool for validators for block production;
    * 20 % goes to reward pool for workers;
    * 70 % goes to the stake pool that is distributed between validators and users that voted for them.  

  * Validator rewards are distributed in the following proportions:  
    * 10 % from the validators pool goes to a validator, as well as commissions that he/she sets, which are taken from the stake pool;  
    * the rest of the stake pool is distributed between the proxy and generic accounts that voted for this specific validator.  


### Requirements and actions applied to a validator

  * Validators must produce blocks in accordance with sequence specified in the schedule. If the schedule contains N vaditors, then N blocks must be produced per one cycle.
  * Validator is not rewarded if no one block was produced by him during one round. The highlighted reward to validators is marked on their special account at the beginning of a round. When a validator produces block, he marks these funds as requested and at the beginning of the next round they are transferred to the validator account. If the validator skips block production, then this award is burned (this is reported using `burnreward` event).
  * If a validator skips 100 rounds (no any block produced during 4×100 cycles) in a row, his key will be reset and a ban on changing the key for one day will be set.
  * If 4 series are being skipped by a validator in a row of 100 rounds (no any block produced during 4×4×100 cycles), the validator will be automatically denounced by setting *proxy_level* to the maximum value and it will be forbidden to return to activity within 7 days.
  * If a validator does not demonstrate any activity within 30 days, then any user can suspend the validator using `suspendcand` action.
 