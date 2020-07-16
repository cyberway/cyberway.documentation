# How To Bay Stake Using CYBER tokens

## Goal
Transfer tokens *CYBER* to stake.

## Steps
The operation can be performed through the contract `cyber.token`.

### Step 1
Go to the page `https://explorer.cyberway.io/account/<account ID>` and specify ID of your account. Let it is *shwojevqcywn*. See balance of this account.  

![](./images/balance_to_bay_stake.png)

The field `Own`contains total amount of staked tokens of this account. In addition, there are another 804,0000 CYBER tokens on balance of this account. This amount can be spent on purchase of stake.

### Step 2
Go to the page `https://explorer.cyberway.io/account/cyber.token/contract`.  

### Step 3
Choose the tab `transfer` and fill in fields.

![](./images/bay_stake.png)

**Fields:**
 * `from` - the identifier of your account.
 * `to`- recipient, the contract `cyber.token`.
 * `quantity` - the number of funds transferred, taking into account the required accuracy. For *CYBER* tokens, you must specify four numbers after the point.
 * `memo` - field is left blank (or your account)if the stake is bought for itself. If the staked tokens are transferred to another account, you have to specify ID of account-recipient. Tokens will be transferred to steak of that account.
 * `authorization` - the same identifier of your account.

### Step 4
Click `Build transaction`.  

### Step 5
Check transaction.  
Make sure the transaction contains correct information.  

### Step 6
Subscribe with your private key and click `Sign transation` to send it on blockchain.
