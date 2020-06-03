# How To Calculate Reward For An Author

### Goal
Сalculate reward to author for a post in Golos application.

### Steps

To calculate total amount of reward for a post you can use the formula:
```
payout = reward_weight × funds × (sharesfn / rsharesfn)
```
  * `payout` — total amount of reward for the post.
  * `reward_weight` — a weight of reward for a post.
  * `funds` — total number of tokens in the reward pool.
  * `sharesfn` — a share of tokens that allocated in the rewards pool to be used for reward for the post (this parameter depends on a weight of the post).
  * `rsharesfn` — number of tokens that allocated in the rewards pool to be be spent on reward for all posts (parameter depends on total weight of all posts).

To determine the amount of reward to author of the post it is recommended to use the formula:
```
author_reward = payout - curation_payout - ben_payout_sum.
```
  * `curation_payout` — total amount of [fee to curators](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition#salculating-the-amount-of-fees-to-curators-for-a-post) .
  * `ben_payout_sum` — total amount of [payments to beneficiaries](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition#salculating-the-rewards-to-beneficiaries-for-a-post) .

### Useful links
  * [Determining Rewards for a Post](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition)
