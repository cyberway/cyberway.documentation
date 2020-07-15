# How To Withdraw Stake

## Goal
Withdraw tokens *CYBER* from stake and make them liquid.

## Steps
The operation can be performed through the contract `cyber.token`.

### Step 1
Go to the page `https://explorer.cyberway.io/account/<account ID>` and specify ID of your account. Let it is *shwojevqcywn*. See balance of this account.  

![](./images/balance_to_bay_stake.png)

The field `Own`contains total amount of staked tokens of this account. A piece of this amount can be withdrawn. Part of the steak is blocked due to using resources.

### Step 2
Go to the page `https://explorer.cyberway.io/account/cyber.token/contract`.  

### Step 3
Choose the tab `withdrawn` and fill in fields.

![](./images/withdraw.png)

In the field `account`, you have specify the identifier of your account.  

In the field `quantity` it is necessary to set the number of funds transferred, taking into account the required accuracy. For *CYBER* tokens, you must specify four numbers after the point.  

In the field `authorization` you must specify the same identifier of your account.

### Step 4
Click `Build transaction`.  

### Step 5
Check transaction.  
Make sure the transaction contains correct information.  

### Step 6
Subscribe with your private key and click `Sign transation` to send it on blockchain.  
Operation is performed instantly.
