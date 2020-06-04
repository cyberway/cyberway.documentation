# How To Calculate Reward For A Curator

### Goal
Сalculate reward to curator for a post in Golos application.

### Steps

Сalculate total amount of fee to curators for the post:
```
curation_payout = curators_prcnt × payout
```
  * `curators_prcnt` — share (in percent), total reward for the post deducted to curators. This percentage is specified by author during the post creation. If author does not specify this percentage, the minimal curators percentage value is taken. 
  * `payout` — [total reward](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition#calculating-a-total-reward-for-a-post) for the post.


To calculate the reward `curator_rewardⱼ` allocated to individual curator j you can use this formula:
```
curator_rewardⱼ = curation_payout × (curatorswⱼ / weights_sum)
```
  * `curatorswⱼ` — a weight of a positive vote j of the «upvote» type.
  * `weights_sum` — total weight of all positive votes of the «upvote» type.
  * `(curatorswⱼ / weights_sum)` — a share allocated to j-th curator of the total fee of all curators.

The more curator have staked tokens, the greater percentage of total reward is allocated to him.

### Useful links
  * [Determining Rewards for a Post](https://docs.cyberway.io/devportal/application_contracts/golos_contracts/rewards_definition)
