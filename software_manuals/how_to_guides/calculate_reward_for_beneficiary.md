# How To Calculate Reward For A Beneficiary

### Goal
Сalculate reward to beneficiary for a post in Golos application.

### Steps

To calculate the reward to a beneficiary, you can apply the formula:
```
ben_reward = (payout - curation_payout) × weight
```
  * `ben_reward` — amount of reward to the beneficiary.
  * `payout` — total amount of reward for the post, calculated by the [formula](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition#calculating-a-total-reward-for-a-post).
  * `curation_payout` — total amount of fee to curators for the post, calculated by the [formula](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition#salculating-the-amount-of-fees-to-curators-for-a-post).
  * `(payout` - curation_payout)` — total amount of rewards allocated to beneficiaries and author.
  * `weight` — a weight of reward allocated to the beneficiary. This percentage value is set by the author at the time of posting.


**Important**  
> The part of funds allocated to author for a post is distributed between the beneficiaries and the author. The share of total reward allocated to beneficiaries, as well as the number of beneficiaries, are determined by the author at the time of posting.

### Useful links
  * [Determining Rewards for a Post](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition)
